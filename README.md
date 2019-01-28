# hive_complex_datatypes

1.ARRAY()

Dataset_Temperature
1/2/17	maha	23.2,12.1,13.2,14.5
2/2/17	mp	23.1,45.5,34.7,45.7
3/2/17	JK	34.5,45.2,56.3,34.5


>create table temperature(date string,city string,mytemp array<double>) 
row format delimited
fields terminated by '\t'
collection items terminated by ',';

>load data local inpath '/home/hadoopuser/RAWFILES/datatemp' into table temperature;

>select * from temperature;

>select city,mytemp[0] from temperature;



2.Map:

Map is a collection of key-value pairs where fields 

are accessed using array notation of keys


Dataset_School_Data:

secondschool	maha	male	2015:56,2016:55,2017:45
secondschool	maha	male	2015:52,2016:50,2017:44	
secondschool	maha	female	2015:56,2016:55,2017:45

>create table myschools(stype string,state string,gender string,total map<int,int>)
row format delimited
fields terminated by '\t'
collection items terminated by ','
map keys terminated by ':' ;


>load data local inpath '/home/hadoopuser/RAWFILES/schooldata' into table myschools;

>select total[2016] from MySchools where state='maha';

>select total[2017] from MySchools where state="Chhattisgarh" and gender="Female";



3.Struct:
Struct is a record type which encapsulates a set of named 
fields that can be any primitive data type. An element 
in STRUCT type can be accessed using the DOT (.) notation.

Dataset_Bikes:
yamaha	aircooled,149.8,13.0,0	
honda		aircooled,14.8,1.0,0
hero	aircooled,19.8,3.0,0


>create table mybike(name string,bikefeature struct<enginetype:string,cc:float,power:float,gears:int>)
row format delimited
fields terminated by '\t'
collection items terminated by ',';

>load data local inpath '/home/hadoopuser/RAWFILES/bikedata' into table mybike;

>select * from mybike;

>select BikeFeatures.EngineType from MyBikes;

>select BikeFeatures.EngineType from MyBikes where name="Suzuki Swish";




********Advance commands

Studentdata:

1,swap,math1$history$science,fmonth#11000$smonth#23000$tmonth#3000,pune$maha$411005
2,swaroop,math$hist$science,fmonth#13000$smonth#22000$tmonth#3000,mum$maha$411005
3,raj,math2$history$sci,fmonth#10200$smonth#21000$tmonth#3000,pune$maha$411005



>create table if not exists student_details(sid int comment 'studentId',
sname string comment 'student name',
subject array<string> comment 'subjects',
pay_details map<string,int> comment 'payment Details',
address struct<city:string,state:string,pin:int> comment 'Address details')
comment 'student_details'
row format delimited       
fields terminated by ','
collection items terminated by '$'
map keys terminated by '#'
stored as textfile
TBLPROPERTIES('CREATOR'='ME','DATE'='11-10-2018','SKIP.HEADER.LINE.COUNT'='1','AUTO.PURGE'='TRUE');

>load data local inpath '/home/hadoopuser/RAWFILES/studentdetails' into table student_details;



