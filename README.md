# Apache hadoop installation steps
## Install OpenJDK Java  and git using below command
* su - root
* yum install java-1.8.0-openjdk.x86_64
* yum install git
## Install and setup Ant
* wget https://archive.apache.org/dist/ant/binaries/apache-ant-1.9.16-bin.tar.gz -P $HOME/
* tar -xvf apache-ant-1.9.16-bin.tar.gz -C $HOME/
## Download the Apache hadoop installation github files
* git clone https://github.com/skumarx87/apache_hadoop_ant_install.git
* cd apache_hadoop_ant_install
* $HOME/apache-ant-1.9.16/bin/ant -f hadoop_install.xml

