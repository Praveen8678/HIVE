# HIVE
Hive End To End

Creating the Table:-- default is Managed Table
*********************

Managed Table:--
-------------
hive> create table emp20(eId int, eName string, eSal int)
    > row format delimited                               
    > fields terminated by '|'                           
    > stored as textfile;    

External Table:--
---------------
hive> create external table emp20(eId int, eName string, eSal int)
    > row format delimited                                        
    > fields terminated by '|'                                    
    > stored as textfile                                          
    > location '/Praveen/HIVE/emp';


Insert the data into the table:--  default is HDFS
**********************************

1. Local --- input file path is from local and output is in HDFS 
	Synx:--load data local inpath '<inputLocalFilePath>' into table <tableName>

2. HDFS  --- input file path is from HDFS and output is in HDFS
	Synx:-- load data inpath '<inputHdfsFilePath>' into table <tableName>


Load:--
	local mode --> copy and paste
	hdfs mode  --> cut and paste

Partition:--
***********

1. Static
2. Dynamic
	i. create the table abc
	ii. load the data into abc
     	iii. crate the dynamic table xyz
	iv. set hive.exec.dynamic.partiton.mode=nonstrict
	v. insert overwrite table xyz partition (<partcol>) select <fileds> from abc;

hive> create table emppart20(eid int, ename string, esal int, eloc string)
    > row format delimited                                                
    > fields terminated by '|'                                            
    > stored as textfile;

hive> load data local inpath 'empDetails1.txt' into table emppart20;

hive> set hive.exec.dynamic.partiton.mode=nonstrict
hive> create table dyncpartemp20 (eid int, ename string, esal int)
    > partitioned by (eloc string)
    > row format delimited                                          
    > fields terminated by '|'                                      
    > stored as textfile;  

hive> insert overwrite table dyncpartemp20           
    > partition (eloc)                               
    > select eid,ename,esal,eloc from emppart20;

Creating the Table:--
*******************
1. New

hive> create table asemp20(ename string, eloc string)
    > row format delimited
    > fields terminated by '|'
    > stored as textfile;
OK
Time taken: 19.636 seconds

2. using existing table (create and load)

hive> create table asExemp20 as select ename,eloc from dyncpar
