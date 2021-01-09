aws ec2 create-snapshots \
 --instance-specification InstanceId=i-0e68a0f2f7cf8b8a2 \
 --tag-specifications 'ResourceType=snapshot,Tags=[{'Key'='MYTIMESTAMP','Value'='2021-01-08-08-52-02'}]' \
 --description "i-0e68a0f2f7cf8b8a2"

aws ec2 describe-snapshots --owner-ids self --filters Name=status,Values=pending
aws ec2 describe-snapshots --owner-ids self --filters Name=tag:MYTIMESTAMP,Values=2021-01-08-08-52-02 Name=status,Values=pending

aws ec2 create-snapshots \
 --instance-specification InstanceId=i-0e68a0f2f7cf8b8a2 \
 --tag-specifications 'ResourceType=snapshot,Tags=[{'Key'='MYTIMESTAMP','Value'="'"$MYVAL"'"}]' \
 --description "i-0e68a0f2f7cf8b8a2"

aws ec2 describe-snapshots --owner-ids self --filters Name=status,Values=pending
aws ec2 describe-snapshots --owner-ids self --filters Name=tag:MYTIMESTAMP,Values=2021-01-08-08-52-02 Name=status,Values=pending
aws ec2 describe-snapshots --owner-ids self --filters Name=tag:MYTIMESTAMP,Values=2021-01-08-08-52-02 Name=status,Values=pending|jq '.[] | type,length'

aws ec2 describe-snapshots --owner-ids self --filters Name=tag:MYTIMESTAMP,Values=2021-01-08-08-52-02 Name=status,Values=pending|jq '.
aws ec2 describe-snapshots --owner-ids self --filters Name=tag:MYTIMESTAMP,Values=2021-01-08-08-52-02 Name=status,Values=pending|jq '.[] | type,length'
aws ec2 describe-snapshots --owner-ids self --filters Name=tag:MYTIMESTAMP,Values=2021-01-08-08-52-02 Name=status,Values=pending|jq '.[] | length'
"array"
0

```
oracle@oracle11g:/home/oracle> cat env.sh
BACKUPLOG=/home/oracle/backup.log

oracle@oracle11g:/home/oracle> cat main.sh
#!/bin/bash
source /home/oracle/env.sh
source /home/oracle/printall.sh
MYDATE=`date "+%Y-%m-%d-%H-%M-%S"`
#export ORACLE_SID=salesdb
PRINTALL 'Starting : Backup Script!!!'
PRINTALL 'Executing : alter database begin backup;'
./beginbackup.sh
PRINTALL 'Completed : alter database begin backup;'
PRINTALL 'Creating : EBS Snapshots in EC2;'
./keepalive.sh $MYDATE
PRINTALL 'Completed : EBS Snapshots in EC2;'
PRINTALL 'Executing : alter database end backup;'
./endbackup.sh
PRINTALL 'Completed : alter database end backup;'
PRINTALL 'Completed : Backup Script!!!'

oracle@oracle11g:/home/oracle> cat beginbackup.sh
#!/bin/bash
export ORACLE_SID=salesdb
sqlplus -s system/Octank#1234 <<EOF
alter database begin backup;
exit;
EOF;

oracle@oracle11g:/home/oracle> cat endbackup.sh
#!/bin/bash
export ORACLE_SID=salesdb
sqlplus system/Octank#1234 <<EOF
alter database backup controlfile to '/data3/controlbackup/control.ctl' reuse ;
alter database backup controlfile to trace '/data3/controlbackup/control.bak' reuse;
alter database end backup;
alter system switch logfile ;
alter system switch logfile ;
exit;
EOF;

```
