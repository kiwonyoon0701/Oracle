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


impdp scott/tiger@db10g tables=EMP,DEPT directory=TEST_DIR dumpfile=EMP_DEPT.dmp logfile=impdpEMP_DEPT.log

```