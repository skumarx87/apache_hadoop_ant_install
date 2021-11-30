# Apache hadoop installation steps
## Setup the SSH Key authentication for non-root account within server
* ssh-keygen -t rsa
* chmod 0700 $HOME/.ssh
* ssh-copy-id -i $HOME/.ssh/id_rsa.pub sathish@localohost
## Install OpenJDK Java  and git using below command
* su - root
* yum install java-1.8.0-openjdk.x86_64
* yum install java-1.8.0-openjdk-devel
* yum install git
## Install and setup ant
* wget https://archive.apache.org/dist/ant/binaries/apache-ant-1.9.16-bin.tar.gz -P $HOME/
* tar -xvf apache-ant-1.9.16-bin.tar.gz -C $HOME/
## Download the Apache hadoop installation github files
* git clone https://github.com/skumarx87/apache_hadoop_ant_install.git

## Update bigdata.properties file with you server details and location ##
* cd apache_hadoop_ant_install
```
bigdata.root=/usr/bigdata
bigdata.user=sathish
namenode.hostname=laksha.home.com
dfs.replication.level=1

##Release version ##
bigdata.release.version=1.0.0

##Hadoop Componets version ##

hadoop.version=3.2.0
hive.version=2.3.5
spark.version=3.0.3
spark.hadoop.version=3.2
tez.version=0.9.2
derby.version=10.10.2.0


##Download URL ##

apache.hadoop.site=https://archive.apache.org/dist
apache.hive.site=https://archive.apache.org/dist
apache.spark.site=https://dlcdn.apache.org/
apache.tez.site=https://dlcdn.apache.org
apache.derby.site=https://archive.apache.org/dist


deploy.local=true
deploy.local.dir=/usr/bigdata/buildtmp/parcel
```
## Create slaves files
* create apache_hadoop_ant_install/conf/slaves file and add the hostname 
* ![image](https://user-images.githubusercontent.com/10299142/144032978-b51f2392-5b0b-433d-9f5a-ad751751a91c.png)

## Run the ant build for hadoop installation
* cd apache_hadoop_ant_install
* export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-2.el8_5.x86_64
* $HOME/apache-ant-1.9.16/bin/ant -f hadoop_install.xml

# Apache hadoop administration
Update the $HOME/.bashrc file with below line
* . /usr/bigdata/Env/1.0.0/scripts/bigdata-user-profile.sh.template
Then logoff and login back

## NameNode Format and Hive metastore initialize
* /usr/bigdata/Env/1.0.0/scripts/install.sh fresh_install
## Managing hadoop services
### stop and start services
```
/usr/bigdata/Env/1.0.0/scripts/install.sh stop
/usr/bigdata/Env/1.0.0/scripts/install.sh start
/usr/bigdata/Env/1.0.0/scripts/install.sh status
```
![image](https://user-images.githubusercontent.com/10299142/144044045-c2df8f26-3fd4-4911-8d20-770924aca6b4.png)
### Basic hdfs admin commands
```
touch /tmp/test.txt
hdfs dfs -mkdir /tmp
hdfs dfs -mkdir /user/
hdfs dfs -copyFromLocal /tmp/test.txt /user/
hdfs dfs -copyToLocal /user/test.txt /tmp/
```
### Testing Spark context
```
import os import pyspark from pyspark.sql import SQLContext, SparkSession

sc = SparkSession
.builder
.master('spark://192.168.198.128:7077')
.appName("sparkFromJupyter")
.getOrCreate()

sqlContext = SQLContext(sparkContext=sc.sparkContext, sparkSession=sc) print("Spark Version: " + sc.version) print("PySpark Version: " + pyspark.version)
```
## Testing Hive 
```
beeline -u "jdbc:hive2://localhost:10000/default"
create database test;
CREATE TABLE test.testTable (id INT,Name STRING);

0: jdbc:hive2://localhost:10000/default> show databases;
+----------------+
| database_name  |
+----------------+
| default        |
| test           |
+----------------+
0: jdbc:hive2://localhost:10000/default> show create table test.testTable;;
+----------------------------------------------------+
|                   createtab_stmt                   |
+----------------------------------------------------+
| CREATE TABLE `test.testTable`(                     |
|   `id` int,                                        |
|   `name` string)                                   |
| ROW FORMAT SERDE                                   |
|   'org.apache.hadoop.hive.serde2.lazy.LazySimpleSerDe'  |
| STORED AS INPUTFORMAT                              |
|   'org.apache.hadoop.mapred.TextInputFormat'       |
| OUTPUTFORMAT                                       |
|   'org.apache.hadoop.hive.ql.io.HiveIgnoreKeyTextOutputFormat' |
| LOCATION                                           |
|   'hdfs://laksha.home.com:9000/data/hive/warehouse/test.db/testtable' |
| TBLPROPERTIES (                                    |
|   'transient_lastDdlTime'='1638275125')            |
+----------------------------------------------------+
13 rows selected (0.406 seconds)
```

## Hadoop URL's
### NameNode URL
https://${hostname}:9871/dfshealth.html
### Spark URL
https://${hostname}:8480/
### YARN URL
https://${hostname}:8088/cluster

