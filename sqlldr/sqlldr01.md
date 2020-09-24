```
SQL> create user bike identified by Octank#1234 default tablespace users temporary tablespace temp quota unlimited on users;
User created.

SQL> grant connect,resource to bike;
Grant succeeded.

SQL> conn bike/Octank#1234
SQL> create table bikeinfo(
tripduration NUMBER,
startime TIMESTAMP,
stoptime TIMESTAMP,
start_station NUMBER,
start_station_name VARCHAR2(100),
start_latitude FLOAT,
start_longtitude FLOAT,
end_station NUMBER,
end_station_name VARCHAR2(100),
end_latitude FLOAT,
end_longtitiude FLOAT,
bikeid NUMBER,
usertype VARCHAR2(15),
birthyear VARCHAR2(10),
gender NUMBER
);

oracle@octank-internal:/u01/sampledata> cat bikeinfo.ctl
load data
infile file1.csv
into table bikeinfo
append
fields terminated by ","
(
tripduration NUMBER external,
startime TIMESTAMP,
stoptime TIMESTAMP,
start_station NUMBER,
start_station_name VARCHAR2(100),
start_latitude FLOAT,
start_longtitude FLOAT,
end_station NUMBER,
end_station_name VARCHAR2(100),
end_latitude FLOAT,
end_longtitiude FLOAT,
bikeid NUMBER,
usertype VARCHAR2(15),
birthyear VARCHAR2(10),
gender NUMBER
)

oracle@octank-internal:/u01/sampledata> sqlldr salesadm/salesadm@salesdb control=bch.ctl bindsize=10485760 rows=10000 skip=1

```
