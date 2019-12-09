---
layout: post
title: How to create a AWS Code Pipeline using AWS Code Commit , Code Build and Code Deploy
description: >

author: author1
canonical_url:
categories: [Programming,DevOps]
#tags: [Programming1 , Numpy1]
---
### How to create a AWS Code Pipeline using AWS Code Commit , Code Build and Code Deploy
**CodePipeline - Introduction**

**AWS CodePipeline** is a continuous delivery service to model, visualize, and automate the steps required to release your software.

**Benefits**

Automation of Build , Test and Release Process.

Pipeline History Reports and Pipeline Status Visualizations

Establish a consistent release process.

Speed up delivery while improving quality.

Supports external tools integration for source, build and deploy.


Below is a high level flow in a code pipeline & description of the key pieces involved .

![](/BeerAndDiapers.ai/images/awscodepipeline/0.png)

**Code Commit** : a fully-managed source control service that hosts secure Git-based repositories.

**Code Build**: a fully managed continuous integration service that compiles source code, runs tests, and produces software packages that are ready to deploy.

**Code Deploy**: a fully managed deployment service that automates software deployments to a variety of compute services such as Amazon EC2, AWS Fargate, AWS Lambda, and your on-premises servers.

**Code Pipeline** :a fully managed continuous delivery service that helps you automate your release pipelines for fast and reliable application and infrastructure updates.

**Use case** : We have a PHP application which we want to use in a scenario where multiple developers work on the same code . As soon as a developer commits the code a new build should be created , tested and deployed automatically

Below are the steps to create a AWS Code pipeline using all the native AWS tools .

-   Step#1: Create a Code Commit Repository
-   Step#2: Create Code Build
-   Step#3: Create Beanstalk App
-   Step#4: Create Code Pipeline
-   Step#5: Create Code Deploy

**Create Code Commit Repository**

Visit Github Repo

[https://github.com/awslabs/aws-codepipeline-s3-aws-codedeploy_linux](https://github.com/awslabs/aws-codepipeline-s3-aws-codedeploy_linux)

Or Link to my repo :  [https://github.com/dogradiwakar/AWS-CICD-Codecommit](https://github.com/dogradiwakar/AWS-CICD-Codecommit)

Select the dist folder & Select the file named aws-codepipeline-s3-aws-codedeploy_linux.zip.

Select View Raw & save the sample file

![](/BeerAndDiapers.ai/images/awscodepipeline/1.png)

For repo access User should have Code commit Full access role assigned to his login .

Clone the Repo

![](/BeerAndDiapers.ai/images/awscodepipeline/2.png)

Connect to your repository , copy sample files

Create the buildspec.yml file and add below content to it . This will be used for initiating the build .

![](/BeerAndDiapers.ai/images/awscodepipeline/3.png)

Push using below git commands .

    git add *

![](/BeerAndDiapers.ai/images/awscodepipeline/4.png)

    git commit –m “v1”

![](/BeerAndDiapers.ai/images/awscodepipeline/5.png)

    git push

![](/BeerAndDiapers.ai/images/awscodepipeline/6.png)

**Create Code Build**

![](/BeerAndDiapers.ai/images/awscodepipeline/7.jpg)

Configure Source

![](/BeerAndDiapers.ai/images/awscodepipeline/8.jpg)

Configure environment
![](/BeerAndDiapers.ai/images/awscodepipeline/9.jpg)

![](/BeerAndDiapers.ai/images/awscodepipeline/10.jpg)

Configure Artifact

![](/BeerAndDiapers.ai/images/awscodepipeline/11.jpg)

Click on Create Build Project

Click on Start Build

![](/BeerAndDiapers.ai/images/awscodepipeline/12.jpg)

Once all the Stages are finished, the Build File will be generated in the S3 Bucket

![](/BeerAndDiapers.ai/images/awscodepipeline/13.jpg)

Check S3 for the File
![](/BeerAndDiapers.ai/images/awscodepipeline/14.jpg)

**Create Beanstalk App**

![](/BeerAndDiapers.ai/images/awscodepipeline/15.jpg)

Click on Upload your code and give path of the S3 File

![](/BeerAndDiapers.ai/images/awscodepipeline/16.png)

Click on Create Application.

**Create Pipeline**

![](/BeerAndDiapers.ai/images/awscodepipeline/17.jpg)

Configure Source

![](/BeerAndDiapers.ai/images/awscodepipeline/18.jpg)

Configure Build

![](/BeerAndDiapers.ai/images/awscodepipeline/19.jpg)

Configure Deploy

![](/BeerAndDiapers.ai/images/awscodepipeline/20.jpg)

Click on Create Pipeline

![](/BeerAndDiapers.ai/images/awscodepipeline/21.png)

Open the Sample App Page , Do some changes to the index.html and again commit the code to the code commit repo .

The Pipeline will get initiated as soon as commit is done to the code .

![](/BeerAndDiapers.ai/images/awscodepipeline/22.png)
