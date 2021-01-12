**main.sh**

```
oracle@oracle11g:/home/oracle> cat main.sh
#!/bin/bash
source /home/oracle/env.sh
source /home/oracle/printall.sh
MYDATE=`date "+%Y-%m-%d-%H-%M-%S"`
PRINTALL 'Starting : Backup Script!!!'
PRINTALL 'Executing : beginbackup.sh'
./beginbackup.sh
PRINTALL 'Starting : Creating EBS Snapshots in EC2;'
./createsnapshots.sh $MYDATE
PRINTALL 'Completed : Creating EBS Snapshots in EC2;'
PRINTALL 'Executing : endbackup.sh'
./endbackup.sh
PRINTALL 'Completed : Backup Script!!!'
```

**beginbackup.sh**

```
oracle@oracle11g:/home/oracle> cat beginbackup.sh
#!/bin/bash
source /home/oracle/env.sh
source /home/oracle/printall.sh
export ORACLE_SID=salesb
PRINTALL 'Starting : Creating EBS Snapshots in EC2;'
sqlplus -s system/PASSWORD <<EOF
alter database begin backup;
exit;
EOF;
PRINTALL 'Completed : Creating EBS Snapshots in EC2;'
```

**endbackup.sh**

```
oracle@oracle11g:/home/oracle> cat endbackup.sh
#!/bin/bash
source /home/oracle/env.sh
source /home/oracle/printall.sh
export ORACLE_SID=salesdb
PRINTALL 'Starting : alter database end backup;'
sqlplus system/PASSWORD <<EOF
alter database backup controlfile to '/data3/controlbackup/control.ctl' reuse ;
alter database backup controlfile to trace as '/data3/controlbackup/control.bak' reuse;
alter database end backup;
alter system switch logfile ;
alter system switch logfile ;
exit;
EOF;
PRINTALL 'Completed : alter database end backup;'
```

**createsnapshots.sh**

```
oracle@oracle11g:/home/oracle> cat createsnapshots.sh
#!/bin/bash
source /home/oracle/env.sh
source /home/oracle/printall.sh
MYTIMESTAMP=$1
PRINTALL 'Starting : Creating EBS Snapshots in EC2;'
aws ec2 create-snapshots \
    --instance-specification InstanceId=i-0e68a0f2f7cf8b8a2 \
    --tag-specifications 'ResourceType=snapshot,Tags=[{'Key'='MYTIMESTAMP','Value'="'"$MYTIMESTAMP"'"}]'   \
    --description "i-0e68a0f2f7cf8b8a2-snapshot" |  jq -c '.Snapshots[] | "\(.VolumeId) \(.SnapshotId)"' >> /home/oracle/backup.log
while true
do
FLAG=`aws ec2 describe-snapshots --owner-ids self --filters Name=tag:MYTIMESTAMP,Values="'"$MYTIMESTAMP"'" Name=status,Values=pending|jq '.[] | length'`
PRINTALL 'Current Number of Pending Snapshots : ' $FLAG
if [ $FLAG = 0 ]
then
break;
fi
date
sleep 10;
done
PRINTALL 'Completed : Creating EBS Snapshots in EC2;'
```

**env.sh**

```
oracle@oracle11g:/home/oracle> cat env.sh
BACKUPLOG=/home/oracle/backup.log
```

**printall.sh**

```
oracle@oracle11g:/home/oracle> cat printall.sh
#!/bin/bash
source /home/oracle/env.sh
function PRINTDATE()
{
        date >> $BACKUPLOG;
}
function PRINTLOG()
{
        echo $@ >> $BACKUPLOG;
}
function PRINTALL()
{
        PRINTDATE;
        PRINTLOG $@;
}
```

1. Create new Instance from AMI
2. Detach old EBS
3. Create new EBS from snapshots(EBS-Snapshot-Mapping in bakcup.log)
4. Attach new EBS into new instance
   1. new EBSs attahced into EC2 automatically
   2. Also mounted as same mount point
5. Copy required archive file and backup controlfile from Source DB to Target DB

```
[ec2-user@SOURCE arc]$ scp -i ~/id.pem ./* ec2-user@10.100.1.226:/tmp/arc/
control.ctl                                                   100% 9712KB 125.1MB/s   00:00
o1_mf_1_42_hznqoc8p_.arc                                      100%  461KB 120.9MB/s   00:00
o1_mf_1_43_hznqqwch_.arc                                      100%  914KB 111.4MB/s   00:00
o1_mf_1_44_hznqqzd4_.arc                                      100%   26KB  30.3MB/s   00:00

oracle@oracle11g:/home/oracle> cp /tmp/arc/control.ctl /data2/salesdb/control01.ctl
oracle@oracle11g:/home/oracle> cp /tmp/arc/control.ctl /data2/salesdb/control02.ctl
oracle@oracle11g:/data3/archivelog/SALESDB/archivelog/2021_01_09> cp /tmp/arc/*.arc .

```

6. Recover database using backup controfile until cancel

```
SQL> startup
ORACLE instance started.

Total System Global Area 9386692608 bytes
Fixed Size                  2261088 bytes
Variable Size            1543507872 bytes
Database Buffers         7818182656 bytes
Redo Buffers               22740992 bytes
Database mounted.
ORA-01589: must use RESETLOGS or NORESETLOGS option for database open

ORA-01547: warning: RECOVER succeeded but OPEN RESETLOGS would get error below
ORA-01195: online backup of file 1 needs more recovery to be consistent
ORA-01110: data file 1: '/data2/salesdb/system01.dbf'

SQL> recover database until cancel using backup controlfile;
ORA-00279: change 1016265 generated at 01/09/2021 20:55:48 needed for thread 1
ORA-00289: suggestion :
/data3/archivelog/SALESDB/archivelog/2021_01_09/o1_mf_1_42_hznqoc8p_.arc
ORA-00280: change 1016265 for thread 1 is in sequence #42


Specify log: {<RET>=suggested | filename | AUTO | CANCEL}

ORA-00279: change 1016279 generated at 01/09/2021 20:55:55 needed for thread 1
ORA-00289: suggestion :
/data3/archivelog/SALESDB/archivelog/2021_01_09/o1_mf_1_43_hznqqwch_.arc
ORA-00280: change 1016279 for thread 1 is in sequence #43
ORA-00278: log file
'/data3/archivelog/SALESDB/archivelog/2021_01_09/o1_mf_1_42_hznqoc8p_.arc' no
longer needed for this recovery


Specify log: {<RET>=suggested | filename | AUTO | CANCEL}

ORA-00279: change 1017814 generated at 01/09/2021 20:57:16 needed for thread 1
ORA-00289: suggestion :
/data3/archivelog/SALESDB/archivelog/2021_01_09/o1_mf_1_44_hznqqzd4_.arc
ORA-00280: change 1017814 for thread 1 is in sequence #44
ORA-00278: log file
'/data3/archivelog/SALESDB/archivelog/2021_01_09/o1_mf_1_43_hznqqwch_.arc' no
longer needed for this recovery


Specify log: {<RET>=suggested | filename | AUTO | CANCEL}

ORA-00279: change 1017883 generated at 01/09/2021 20:57:19 needed for thread 1
ORA-00289: suggestion :
/data3/archivelog/SALESDB/archivelog/2021_01_10/o1_mf_1_45_%u_.arc
ORA-00280: change 1017883 for thread 1 is in sequence #45
ORA-00278: log file
'/data3/archivelog/SALESDB/archivelog/2021_01_09/o1_mf_1_44_hznqqzd4_.arc' no
longer needed for this recovery


Specify log: {<RET>=suggested | filename | AUTO | CANCEL}
cancel
Media recovery cancelled.
SQL> alter database open resetlogs;

Database altered.

SQL> exit
Disconnected from Oracle Database 11g Enterprise Edition Release 11.2.0.4.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options
oracle@oracle11g:/home/oracle> sqlplus oshop/oshop

SQL*Plus: Release 11.2.0.4.0 Production on Sun Jan 10 00:54:54 2021

Copyright (c) 1982, 2013, Oracle.  All rights reserved.


Connected to:
Oracle Database 11g Enterprise Edition Release 11.2.0.4.0 - 64bit Production
With the Partitioning, OLAP, Data Mining and Real Application Testing options

SQL> select count(*) from dummy;

  COUNT(*)
----------
       778


```
