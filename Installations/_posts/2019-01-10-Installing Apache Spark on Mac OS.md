---
layout: post
title: Installing Apache Spark on Mac OS
description: >

author: author1
canonical_url:
categories: [Installations]
tags: [Installations]
---

**Pre-requisites**

**Install Brew**

Go to Brew Website
[https://brew.sh/](https://brew.sh/)



Copy the url from Home page on mac os terminal to install Home brew



Run below command to update homebrew

    brew upgrade && brew update

**Java Installation**

Check Java Version

    Java -version


Run below command to install Java8

    brew cask install java8


For Latest Java use

    brew cask install java

Check Java Version



 **Install xcode-select**

    xcode-select --install

**Install Scala**

    brew install scala



Use `scala -version` to get the scala version




**Install Apache Spark**

    brew install apache-spark



To start spark shell execute below command

    Spark-shell


Run below command to check the execution which will return the string "Hello World"

    val s = "hello world"



Run `pyspark` to start pyspark shell



**Add Spark path to bash profile**
Run below command and then add the path to the profile

    nano ~/.profile

    export SPARK_HOME=/usr/local/Cellar/apache-spark/2.4.4/libexec
    export PYTHONPATH=/usr/local/Cellar/apache-spark/2.4.4/libexec/python/:$PYTHONP$
    source ~/.bash_profile





    cd /usr/local/Cellar/apache-spark/2.4.4/libexec/sbin

And execute below command to start all services

    sbin/start-all.sh



Spark Master UI : [http://localhost:8080/](http://localhost:8080/)



Spark Application UI : [http://localhost:4040/](http://localhost:4040/)
