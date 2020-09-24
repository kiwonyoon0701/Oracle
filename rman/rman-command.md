```

oracle@oracle11g:/home/oracle> rman target /
RMAN> list expired archivelog all;

using target database control file instead of recovery catalog
specification does not match any archived log in the repository

RMAN>  crosscheck archivelog all;

allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=5967 device type=DISK
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_31_hlnot3cl_.arc RECID=30 STAMP=1047610390
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_32_hlnp6pno_.arc RECID=31 STAMP=1047610790
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_33_hlnpc4wl_.arc RECID=32 STAMP=1047610924
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_34_hlnplfc5_.arc RECID=33 STAMP=1047611161
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_35_hoklhspq_.arc RECID=36 STAMP=1050654447
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_36_hoklhsqk_.arc RECID=38 STAMP=1050654447
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_37_hoklhsqd_.arc RECID=37 STAMP=1050654447
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_38_hoklhtqq_.arc RECID=35 STAMP=1050654447
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_39_hoklhtqf_.arc RECID=34 STAMP=1050654401
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_40_hoklkhvf_.arc RECID=39 STAMP=1050654448
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_11/o1_mf_1_41_hoq1ft01_.arc RECID=40 STAMP=1050833531
validation succeeded for archived log
archived log file name=/u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_19/o1_mf_1_42_hpfgtn2n_.arc RECID=41 STAMP=1051568157
Crosschecked 12 objects

RMAN> list expired archivelog all;

specification does not match any archived log in the repository

RMAN> delete expired archivelog all;

released channel: ORA_DISK_1
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=5967 device type=DISK
specification does not match any archived log in the repository
```

```
root@oracle11g:/u01/app/oracle/fast_recovery_area/SALESDB/archivelog# rm -rf *
```

```
RMAN> list expired archivelog all;

List of Archived Log Copies for database with db_unique_name SALESDB
=====================================================================

Key     Thrd Seq     S Low Time
------- ---- ------- - ---------
30      1    31      X 04-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_31_hlnot3cl_.arc

31      1    32      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_32_hlnp6pno_.arc

32      1    33      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_33_hlnpc4wl_.arc

33      1    34      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_34_hlnplfc5_.arc

36      1    35      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_35_hoklhspq_.arc

38      1    36      X 14-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_36_hoklhsqk_.arc

37      1    37      X 25-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_37_hoklhsqd_.arc

35      1    38      X 01-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_38_hoklhtqq_.arc

34      1    39      X 01-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_39_hoklhtqf_.arc

39      1    40      X 07-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_40_hoklkhvf_.arc

40      1    41      X 09-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_11/o1_mf_1_41_hoq1ft01_.arc

41      1    42      X 11-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_19/o1_mf_1_42_hpfgtn2n_.arc


RMAN> delete expired archivelog all;

released channel: ORA_DISK_1
allocated channel: ORA_DISK_1
channel ORA_DISK_1: SID=5967 device type=DISK
List of Archived Log Copies for database with db_unique_name SALESDB
=====================================================================

Key     Thrd Seq     S Low Time
------- ---- ------- - ---------
30      1    31      X 04-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_31_hlnot3cl_.arc

31      1    32      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_32_hlnp6pno_.arc

32      1    33      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_33_hlnpc4wl_.arc

33      1    34      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_08_05/o1_mf_1_34_hlnplfc5_.arc

36      1    35      X 05-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_35_hoklhspq_.arc

38      1    36      X 14-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_36_hoklhsqk_.arc

37      1    37      X 25-AUG-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_37_hoklhsqd_.arc

35      1    38      X 01-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_38_hoklhtqq_.arc

34      1    39      X 01-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_39_hoklhtqf_.arc

39      1    40      X 07-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_09/o1_mf_1_40_hoklkhvf_.arc

40      1    41      X 09-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_11/o1_mf_1_41_hoq1ft01_.arc

41      1    42      X 11-SEP-20
        Name: /u01/app/oracle/fast_recovery_area/SALESDB/archivelog/2020_09_19/o1_mf_1_42_hpfgtn2n_.arc


Do you really want to delete the above objects (enter YES or NO)? Yes


```
