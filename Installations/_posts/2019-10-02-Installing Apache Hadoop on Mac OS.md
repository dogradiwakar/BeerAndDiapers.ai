---
layout: post
title: Installing Hadoop on Mac OS
description: >

author: author1
canonical_url:
categories: [Installations]
tags: [Installations]
---
![](/beeranddiaper.com/images/installingHadoppOnMacos/21.png)

**Pre-requisites**

***Install Brew***

Go to Brew Website

[https://brew.sh/](https://brew.sh/)

![](/beeranddiaper.com/images/installingHadoppOnMacos/1.png)

Copy the url from Home page on mac os terminal to install Home brew

![](/beeranddiaper.com/images/installingHadoppOnMacos/2.png)


**Java Installation**

Check Java Version
```
Java -version
```
![](/beeranddiaper.com/images/installingHadoppOnMacos/3.png)


Check below url for supported versions
[https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions](https://cwiki.apache.org/confluence/display/HADOOP/Hadoop+Java+Versions)

![](/beeranddiaper.com/images/installingHadoppOnMacos/4.png)
Run below command to install Java8 as it is supported by Hadoop 3.0
```
brew cask install java8
```
![](/beeranddiaper.com/images/installingHadoppOnMacos/5.png)
For Latest Java use

```
brew cask install java
```
Check Java Version
![](/beeranddiaper.com/images/installingHadoppOnMacos/6.png)


**Install Hadoop using brew**

Run below command
```
Brew install hadoop
```


![](/beeranddiaper.com/images/installingHadoppOnMacos/7.png)

![](/beeranddiaper.com/images/installingHadoppOnMacos/8.png)

**Configuration Changes**

Go to  /usr/local/Cellar/hadoop/3.2.1 and make some changes or create the following files

	1. hadoop-env.sh
	2. core-site.xml
	3. mapred-site.xml
	4. hdfs-site.xml

**hadoop-env.sh**
In  /usr/local/Cellar/hadoop/3.2.1/libexec/etc/hadoop/hadoop-env.sh  look for export JAVA_HOME and configure it to the Java home value
Run below command to get the Java home

    /usr/libexec/java_home



![](/beeranddiaper.com/images/installingHadoppOnMacos/9.png)

![](/beeranddiaper.com/images/installingHadoppOnMacos/10.png)


**core-site.xml**

In core-site.xml, you will configure the HDFS address and port number.
```
<!-- Put site-specific property overrides in this file. -->
<configuration>
  <property>
    <name>hadoop.tmp.dir</name>
    <value>/usr/local/Cellar/hadoop/hdfs/tmp</value>
    <description>A base for other temporary directories</description>             
  </property>
  <property>
    <name>fs.default.name</name>
    <value>hdfs://localhost:8020</value>
  </property>
</configuration>
```

![](/beeranddiaper.com/images/installingHadoppOnMacos/11.png)



**mapred-site.xml**


Add the following into mapred-site.xml .
```
<configuration>
  <property>
    <name>mapred.job.tracker</name>
    <value>localhost:8021</value>
  </property>
</configuration>
```

![](/beeranddiaper.com/images/installingHadoppOnMacos/12.png)


**hdfs-site.xml**

In hdfs-site.xml ,add below
```
<configuration>
  <property>
    <name>dfs.replication</name>
    <value>1</value>
  </property>
</configuration>
```
![](/beeranddiaper.com/images/installingHadoppOnMacos/13.png)


**Configure SSH**

Check if ssh is enabled using below command

    ssh localhost

In case of below error configure ssh

![](/beeranddiaper.com/images/installingHadoppOnMacos/14.png)

Run below commands and Authorize SSH Keys i.e allow your system to accept login, we have to make it aware of the keys that will be used
```
 ssh-keygen -t rsa -P '' -f ~/.ssh/id_rsa
 cat ~/.ssh/id_rsa.pub >> ~/.ssh/authorized_keys
 chmod 0600 ~/.ssh/authorized_keys
```
Enable Remote Login: “System Preferences” -> “Sharing”. Check “Remote Login”

Check ssh again using ssh localhost

![](/beeranddiaper.com/images/installingHadoppOnMacos/15.png)


**Format HDFS**

Finally, the last step before starting to launch the different services would be to format the HDFS.

    cd /usr/local/opt/hadoop

    hdfs namenode -format

![](/beeranddiaper.com/images/installingHadoppOnMacos/16.png)

![](/beeranddiaper.com/images/installingHadoppOnMacos/17.png)

Alias to start and stop Hadoop Daemons

Now, we need to go to /usr/local/Cellar/hadoop/3.2.1/sbin/ to start and stop hadoop services.

Edit ~/.bash_profile and add
```
nano ~/.profile

alias hstart="/usr/local/Cellar/hadoop/3.2.1/sbin/start-all.sh"

alias hstop="/usr/local/Cellar/hadoop/3.2.1/sbin/stop-all.sh"
```
Then run
```
source ~/.bash_profile
```

Manual starting

HDFS Services
Go to /usr/local/opt/hadoop/sbin , there you can use the following scripts

**To start hdfs service**
```
$ ./start-dfs.sh# To stop HDFS service
$ ./stop-dfs.sh
```
**To start all services**
```
$ ./start-all.sh# to stop all services
$ ./stop-all.sh
```

Start Hadoop using the hstart alias
![](/beeranddiaper.com/images/installingHadoppOnMacos/18.png)



Run JPS to check the services

![](/beeranddiaper.com/images/installingHadoppOnMacos/19.png)

Access Hadoop web interface by connecting to

Resource Manager:  [http://localhost:9870](http://localhost:9870)

JobTracker: [http://localhost:8088/](http://localhost:8088/)

Node Specific Info:  [http://localhost:8042/](http://localhost:8042/)

Name Node

![](/beeranddiaper.com/images/installingHadoppOnMacos/20.png)

![](/beeranddiaper.com/images/installingHadoppOnMacos/21.png)
