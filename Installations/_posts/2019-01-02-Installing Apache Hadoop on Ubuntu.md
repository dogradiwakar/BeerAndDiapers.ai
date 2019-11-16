---
layout: post
title: Installing Hadoop on Ubuntu
description: >

author: author1
canonical_url:
categories: [Installations]
tags: [Installations]
---


**Pre-requisites**

Java
```
sudo apt-get install default-jre
```

![](/BeerAndDiapers.ai/images/2018/installinghadoop/1.png)

![](/BeerAndDiapers.ai/images/2018/installinghadoop/2.png)
```
sudo apt-get install default-jdk
sudo apt-get install openjdk-8-jdk
```
![](/BeerAndDiapers.ai/images/2018/installinghadoop/3.png)

**SSH**
```
sudo apt-get install ssh

sudo apt-get install sshd
```
![](/BeerAndDiapers.ai/images/2018/installinghadoop/4.png)

Install Ssh
![](/BeerAndDiapers.ai/images/2018/installinghadoop/5.png)

![](/BeerAndDiapers.ai/images/2018/installinghadoop/6.png)

**Configure SSH**
```
ssh-keygen -t rsa -P ""
```

The second command adds the newly created key to the list of authorized keys so that Hadoop can use ssh without prompting for a password.

```
cat $HOME/.ssh/id_rsa.pub >> $HOME/.ssh/authorized_keys
```
![](/BeerAndDiapers.ai/images/2018/installinghadoop/7.png)

![](/BeerAndDiapers.ai/images/2018/installinghadoop/8.png)

**Install Hadoop**
```
wget http://mirrors.sonic.net/apache/hadoop/common/hadoop-2.9.1/hadoop-2.9.1.tar.gz
```
![](/BeerAndDiapers.ai/images/2018/installinghadoop/9.png)
```
tar xvzf hadoop-2.9.1.tar.gz
```

Move the Hadoop installation to the /usr/local/hadoop directory using the following command:
```
sudo mv * /usr/local/hadoop

sudo chown -R diwakar:diwakar /usr/local/hadoop
```

**Setup Configuration Files**
The following files will have to be modified to complete the Hadoop setup:
	• ~/.bashrc

	• /usr/local/hadoop/etc/hadoop/hadoop-env.sh

	• /usr/local/hadoop/etc/hadoop/core-site.xml

	• /usr/local/hadoop/etc/hadoop/mapred-site.xml.template

	• /usr/local/hadoop/etc/hadoop/hdfs-site.xml



**1. ~/.bashrc:**
Before editing the .bashrc file in our home directory, we need to find the path where Java has been installed to set the JAVA_HOME environment variable using the following command:
```
update-alternatives --config java
```
/usr/lib/jvm/java-8-openjdk-amd64/jre/bin/java

![](/BeerAndDiapers.ai/images/2018/installinghadoop/10.png)



Now we can append the following to the end of ~/.bashrc:
Execute : nano ~/.bashrc

```
#HADOOP VARIABLES START
export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
export HADOOP_INSTALL=/usr/local/hadoop
export PATH=$PATH:$HADOOP_INSTALL/bin
export PATH=$PATH:$HADOOP_INSTALL/sbin
export HADOOP_MAPRED_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_HOME=$HADOOP_INSTALL
export HADOOP_HDFS_HOME=$HADOOP_INSTALL
export YARN_HOME=$HADOOP_INSTALL
export HADOOP_COMMON_LIB_NATIVE_DIR=$HADOOP_INSTALL/lib/native
export HADOOP_OPTS="-Djava.library.path=$HADOOP_INSTALL/lib"
#HADOOP VARIABLES END
```

Execute below command after editing the bashsrc
```
source ~/.bashrc
```

note that the JAVA_HOME should be set as the path just before the '.../bin/':
```
$ javac -version

$ which javac

$ readlink -f /usr/bin/javac
```
![](/BeerAndDiapers.ai/images/2018/installinghadoop/11.png)
```
/usr/lib/jvm/java-8-openjdk-amd64/bin/javac
```


**2. /usr/local/hadoop/etc/hadoop/hadoop-env.sh**

We need to set JAVA_HOME by modifying hadoop-env.sh file.
```
nano  /usr/local/hadoop/etc/hadoop/hadoop-env.sh

export JAVA_HOME=/usr/lib/jvm/java-8-openjdk-amd64
```
Adding the above statement in the hadoop-env.sh file ensures that the value of JAVA_HOME variable will be available to Hadoop whenever it is started up.

**3. /usr/local/hadoop/etc/hadoop/core-site.xml:**

The /usr/local/hadoop/etc/hadoop/core-site.xml file contains configuration properties that Hadoop uses when starting up. 
This file can be used to override the default settings that Hadoop starts with.
```
sudo mkdir -p /app/hadoop/tmp

sudo chown diwakar:diwakar /app/hadoop/tmp
```
Open the file and enter the following in between the <configuration></configuration> tag:
```
nano /usr/local/hadoop/etc/hadoop/core-site.xml
```
```
<configuration>
 <property>
  <name>hadoop.tmp.dir</name>
  <value>/app/hadoop/tmp</value>
  <description>A base for other temporary directories.</description>
 </property>
<property>
  <name>fs.default.name</name>
  <value>hdfs://localhost:54310</value>
  <description>The name of the default file system.  A URI whose
  scheme and authority determine the FileSystem implementation.  The
  uri's scheme determines the config property (fs.SCHEME.impl) naming
  the FileSystem implementation class.  The uri's authority is used to
  determine the host, port, etc. for a filesystem.</description>
 </property>
</configuration>
```


**4. /usr/local/hadoop/etc/hadoop/mapred-site.xml**

By default, the /usr/local/hadoop/etc/hadoop/ folder contains 

/usr/local/hadoop/etc/hadoop/mapred-site.xml.template 

file which has to be renamed/copied with the name mapred-site.xml:


```
$ cp /usr/local/hadoop/etc/hadoop/mapred-site.xml.template /usr/local/hadoop/etc/hadoop/mapred-site.xml
```
```
nano /usr/local/hadoop/etc/hadoop/mapred-site.xml
```

The mapred-site.xml file is used to specify which framework is being used for MapReduce.

We need to enter the following content in between the <configuration></configuration> tag:

```
<configuration>
 <property>
  <name>mapred.job.tracker</name>
  <value>localhost:54311</value>
  <description>The host and port that the MapReduce job tracker runs
  at.  If "local", then jobs are run in-process as a single map
  and reduce task.
  </description>
 </property>
</configuration>
```

**5. /usr/local/hadoop/etc/hadoop/hdfs-site.xml**

The /usr/local/hadoop/etc/hadoop/hdfs-site.xml file needs to be configured for each host in the cluster that is being used. 
It is used to specify the directories which will be used as the namenode and the datanode on that host.

Before editing this file, we need to create two directories which will contain the namenode and the datanode for this Hadoop installation. 
This can be done using the following commands:

```
sudo mkdir -p /usr/local/hadoop_store/hdfs/namenode
sudo mkdir -p /usr/local/hadoop_store/hdfs/datanode
sudo chown -R diwakar:diwakar /usr/local/hadoop_store
```

Open the file and enter the following content in between the <configuration></configuration> tag:

```
nano /usr/local/hadoop/etc/hadoop/hdfs-site.xml
```
```
<configuration>
 <property>
  <name>dfs.replication</name>
  <value>1</value>
  <description>Default block replication.
  The actual number of replications can be specified when the file is created.
  The default is used if replication is not specified in create time.
  </description>
 </property>
 <property>
   <name>dfs.namenode.name.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/namenode</value>
 </property>
 <property>
   <name>dfs.datanode.data.dir</name>
   <value>file:/usr/local/hadoop_store/hdfs/datanode</value>
 </property>
</configuration>
```

Format the New Hadoop Filesystem
Now, the Hadoop file system needs to be formatted so that we can start to use it. The format command should be issued with write permission since it creates current directory 
```
Cd /usr/local/hadoop_store/hdfs/namenode 

hadoop namenode -format
```
![](/BeerAndDiapers.ai/images/2018/installinghadoop/12.png)

There will be files created under current folder
![](/BeerAndDiapers.ai/images/2018/installinghadoop/13.png)

![](/BeerAndDiapers.ai/images/2018/installinghadoop/14.png)


Starting Hadoop
Now it's time to start the newly installed single node cluster.
 
We can use start-all.sh or (start-dfs.sh and start-yarn.sh)
```
cd /usr/local/hadoop/sbin

/usr/local/hadoop/sbin$ start-all.sh
```
![](/BeerAndDiapers.ai/images/2018/installinghadoop/15.png)

![](/BeerAndDiapers.ai/images/2018/installinghadoop/16.png)
```
Jps
```

![](/BeerAndDiapers.ai/images/2018/installinghadoop/17.png)

Web interface

web UI of the NameNode daemon

 : http://localhost:50070
 http://localhost:9870 for recent versions

![](/BeerAndDiapers.ai/images/2018/installinghadoop/18.png)



SecondaryNameNode http://localhost:50090/
http://localhost:9868 for recent versions


[docs]: ../../docs/README.md
