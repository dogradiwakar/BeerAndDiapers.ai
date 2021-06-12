---
layout: post
title: Installing Apache Spark on Mac OS2
description: >

author: author1
canonical_url:
categories: [Installations]
tags: [Installations]
---
![](/images/apachesparkonmacos/11.png)

**Pre-requisites**

**Install Brew**

Go to Brew Website
[https://brew.sh/](https://brew.sh/)

![](/BeerAndDiapers.ai/images/apachesparkonmacos/1.png)

Copy the url from Home page on mac os terminal to install Home brew

![](/images/apachesparkonmacos/2.png)

Run below command to update homebrew

    brew upgrade && brew update

**Java Installation**

Check Java Version

    Java -version

![](/images/apachesparkonmacos/3.png)

Run below command to install Java8

    brew cask install java8

![](/images/apachesparkonmacos/4.png)

For Latest Java use

    brew cask install java

Check Java Version

![](/images/apachesparkonmacos/5.png)


 **Install xcode-select**

    xcode-select --install

**Install Scala**

    brew install scala

![](/images/apachesparkonmacos/6.png)

Use `scala -version` to get the scala version

![](/images/apachesparkonmacos/7.png)


**Install Apache Spark**

    brew install apache-spark

![](/images/apachesparkonmacos/8.png)

To start spark shell execute below command

    Spark-shell

![](/images/apachesparkonmacos/9.png)

Run below command to check the execution which will return the string "Hello World"

    val s = "hello world"

![](/images/apachesparkonmacos/10.png)

Run `pyspark` to start pyspark shell

![](/images/apachesparkonmacos/11.png)

**Add Spark path to bash profile**


Run below command and then add the path to the profile

    nano ~/.profile

    export SPARK_HOME=/usr/local/Cellar/apache-spark/2.4.4/libexec
    export PYTHONPATH=/usr/local/Cellar/apache-spark/2.4.4/libexec/python/:$PYTHONP$
    source ~/.bash_profile

![](/images/apachesparkonmacos/12.png)

    cd /usr/local/Cellar/apache-spark/2.4.4/libexec/sbin

And execute below command to start all services

    sbin/start-all.sh

![](/images/apachesparkonmacos/13.png)

Spark Master UI : [http://localhost:8080/](http://localhost:8080/)

![](/images/apachesparkonmacos/14.png)

Spark Application UI : [http://localhost:4040/](http://localhost:4040/)

![](/images/apachesparkonmacos/15.png)
