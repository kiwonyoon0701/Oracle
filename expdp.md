```
oracle@oracle11g:/home/oracle> mkdir -p swingbench^
oracle@oracle11g:/home/oracle> sqlplus / as sysdba

SQL> CREATE OR REPLACE DIRECTORY swingbench_dir AS '/home/oracle/swingbench';
Directory created.

SQL> GRANT READ, WRITE ON DIRECTORY swingbench_dir to swingbench;
Grant succeeded.

SQL> exit

oracle@oracle11g:/home/oracle> expdp swingbench directory=swingbench_dir dumpfile=swingbench.dmp logfile=swingbench`date "+%Y-%m-%d-%H-%M-%S"`.log

Starting "SWINGBENCH"."SYS_EXPORT_SCHEMA_01":  swingbench/******** directory=swingbench_dir dumpfile=swingbench.dmp logfile=swingbench2020-08-05-00-47-32.log
******************************************************************************
******************************************************************************
  /home/oracle/swingbench/swingbench.dmp
Job "SWINGBENCH"."SYS_EXPORT_SCHEMA_01" successfully completed at Wed Aug 5 00:47:56 2020 elapsed 0 00:00:22

oracle@oracle11g:/home/oracle> ss
SQL> create user swingbench2 identified by <PASSWORD> default tablespace soe temporary tablespace temp quota unlimited on soe;
User created.

SQL> grant connect,resource to swingbench2;
Grant succeeded.

SQL> grant WRITE,READ ON DIRECTORY swingbench_dir to swingbench2;
Grant succeeded.


oracle@oracle11g:/home/oracle> impdp swingbench2 directory=swingbench_dir dumpfile=swingbench logfile=impdp_swingbench2_`date "+%Y-%m-%d-%H-%M-%S"`.log REMAP_SCHEMA=swingbench:swingbench2
***************************************************************************************************************
Job "SWINGBENCH2"."SYS_IMPORT_FULL_01" completed with 4 error(s) at Wed Aug 5 02:55:51 2020 elapsed 0 00:04:46

oracle@oracle11g:/home/oracle> impdp swingbench2 directory=swingbench_dir dumpfile=swingbench logfile=impdp_swingbench2_`date "+%Y-%m-%d-%H-%M-%S"`.log REMAP_SCHEMA=swingbench:swingbench2 parallel=8
***************************************************************************************************************
Job "SWINGBENCH2"."SYS_IMPORT_FULL_01" completed with 4 error(s) at Wed Aug 5 03:02:26 2020 elapsed 0 00:03:44



oracle@oracle11g:/home/oracle> impdp swingbench2 directory=swingbench_dir dumpfile=swingbench logfile=impdp_swingbench2_`date "+%Y-%m-%d-%H-%M-%S"`.log REMAP_SCHEMA=swingbench:swingbench2 parallel=8 ACCESS_METHOD=DIRECT_PATH
***************************************************************************************************************
Job "SWINGBENCH2"."SYS_IMPORT_FULL_01" completed with 4 error(s) at Wed Aug 5 03:06:17 2020 elapsed 0 00:02:27


```

drop user swingbench2 cascade;
create user swingbench2 identified by swingbench2 default tablespace soe temporary tablespace temp quota unlimited on soe;
grant connect,resource to swingbench2;
grant WRITE,READ ON DIRECTORY swingbench_dir to swingbench2;

ORDER_ID
