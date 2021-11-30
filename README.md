# Apache hadoop installation steps
## Install OpenJDK Java  and git using below command
* su - root
* yum install java-1.8.0-openjdk.x86_64
* yum install java-1.8.0-openjdk-devel
* yum install git
## Install and setup Ant
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
## Run the ant build for hadoop installation
* cd apache_hadoop_ant_install
* export JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk-1.8.0.312.b07-2.el8_5.x86_64
* $HOME/apache-ant-1.9.16/bin/ant -f hadoop_install.xml
