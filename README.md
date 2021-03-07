# Vanderbilt - Big Data 2021 - Homework 4 timeseries database - InfluxDB

This is the fourth homework. The deadline is March TBD, 11:59 PM.

# Goals of the exercise. 

* Learn to configure as AWS EC2 instance and install a timeseries DB used in Bigdata systems taking InfluxDB as an example.
* Learn to configure and load InfluxDB with the Nashville tweet Data. 
* Write queries.

# Checklist

+ [Step-1 Create the EC2 Instance](#step-1-create-the-ec2-instance)
+ [Step-2 Install the InfluxDB](#step-2-install-the-influxDB)
+ [Step-3 Configure the InfluxDB](#step-3-configure-the-influxDB)
+ [Step-4 Loading the influxDB with Nashville dataset](#step4-loading-the-influxDB-with-Nashville-dataset)
+ [Step-5 Check initial Colab Connection](#step-5-check-initial-colab-connection)
+ [Step-6 Queries - 30 points-](#step-6-queries---30-points-)




# Background Material

## Prior Reading

* Read all the reading materials given by Prof. Dubey
* Complete the tutorials in week  content.
* Additional links: https://docs.influxdata.com/influxdb/v1.8/introduction/get-started/

## Useful Instructions -- Please watch. Most of your problems will be answered here.

* In the assignment you will create an EC2 instance and install your own influxdb server. For this you will need to see the following videos
  * [Creating and EC2 Instance](https://brightspace.vanderbilt.edu/d2l/le/content/269528/viewContent/1714850/View)
  * [Installing InfluxDB on the EC2 Instance PDF](https://brightspace.vanderbilt.edu/d2l/le/content/269528/viewContent/1714861/View)
  * It is very important to take authentication seriously. Change the password that provide you admin access and set it to your own.


## We have also created some youtube videos that may help you in addition to the links above if you get stuck

* https://brightspace.vanderbilt.edu/d2l/le/content/269528/viewContent/1714860/View




## Remember to update the links of all notebooks

* You did this in previous assignments - see [AcceptingaGithubassignment.pdf](AcceptingaGithubassignment.pdf)


## AWS

To access AWS go to https://aws.amazon.com/education/awseducate/ and use the account you created when you were invited to the class. Ensure that you can access this account and can land into an AWS console as shown below.

## Github

To push code to your repo use the git commit and push commands. But first set some settings:

	git config --global user.name "Your Name"
	git config --global user.email you@example.com

Once you modify files, use git's add, commit and push commands to push files to your repo. 

	git add file.txt
	git commit -a -m 'commit message'
	git push origin master

If you would like to use SSH keys on Github, follow the instructions at:

	https://help.github.com/articles/generating-a-new-ssh-key-and-adding-it-to-the-ssh-agent/0


## Adding Master of original repository

	git clone https://github.com/vu-topics-in-big-data-2021/homework-3-<GITHUB USERNAME>.git I may push updates to this homework assignment in the future. To setup an upstream repo, do the following:

	git remote add upstream https://github.com/vu-topics-in-big-data-2021/homework-3-mongodb.git  
	
	To pull updates do the following: git fetch upstream git merge upstream/master
	
* This will create a new branch: upstream/main
```
	git fetch upstream
```
* Then merge your current brach(origin/main) with upstream/main
```	
	git merge upstream/main --allow-unrelated-histories
```

You will need to resolve conflicts if they occur. If the conflicts are in notebooks. Open them in visual code and manually move the changes.

# Assignment

## Dataset Description

The dataset is Nashville tweet dataset.
 
## Step-1 Create the EC2 Instance

First install the AWS EC2 using the AWS link below (use AWS free educate account login)

Note : In Step 3 :-Configure your instance . Choose Ubuntu server instead of Amazon linux.
Note : Dont do Step 5 as it will terminate your AWS EC2 instance
https://aws.amazon.com/getting-started/tutorials/launch-a-virtual-machine/?trk=gs_card&e=gs&p=gsrc

Caution: After doing your assignment make sure to shut down the EC2 instance and logout. This is necessary to avoid unnecessary charging to your AWS account.
Follow the instructions carefully to remain within **free tier**. That last part is very important.


## Step-2 Install the InfluxDB

Import the public key used for accessing package management system

	$sudo curl -sL https://repos.influxdata.com/influxdb.key | sudo apt-key add -


Create a list file for influxdb
	
	$sudo echo "deb https://repos.influxdata.com/ubuntu bionic stable" | sudo tee /etc/apt/sources.list.d/influxdb.list

	$sudo apt-get update

	$sudo apt install influxdb

Start the mongodb:

	$sudo service mongod start

Verify the mongod service
	
	$sudo service mongod status
	
## Step-3 Configure the influxDB

Edit the /etc/influxdb/influxdb.conf using the any editor, nano is shown below
	
	sudo nano /etc/influxdb/influxdb.conf
	
Uncomment the ‘enable = true’ as shown below and save the influxdb.conf file

	[http]
	# Determines whether HTTP endpoint is enabled.
	enabled = true
	# Determines whether the Flux query endpoint is enabled.
	# flux-enabled = false
	

	
Open the port 8086 in AWS EC2 instance VPC security inbound rules for remote access

	refer https://docs.aws.amazon.com/vpc/latest/userguide/VPC_SecurityGroups.html
	
and restart the influxdb 
	
	$sudo systemctl restart influxdb
	
Type the following command to create user and passwd

	$curl -XPOST "http://localhost:8086/query" --data-urlencode "q=CREATE USER admin WITH PASSWORD 'type_password_here' WITH ALL PRIVILEGES"
	
**Note** - Take security of your database seriously and create a strong password 



## Step-4 Loading the influxDB with Nashville dataset

Type influx at the shell to enter the influx db shell

	$influx -username 'admin' -password 'your_password_here'
	
Create a tweet database at influxdb prompt

	> create database tweet
	
Select the database tweet to use in influxdb

	> use database tweet
	
nashville-tweets-2019-01-28 tweet dataset is in the dataset folder of this assignment
	
edit the test_connection_and_load_tweets.ipynb in the below line to point to the correct path
of nashville-tweets-2019-01-28

	f = open("/content/nashville-tweets-2019-01-28",)


## Step-5 Check initial Colab Connection

Then execute the test_connection_and_load_tweets.ipynb
in colab  to load the nashville tweets database into AWS influxDB:


## Step-6 Queries - 80 points- 

Implement a function per query in a file called [hw4.ipynb](hw4.ipynb). Record the answers there and save it back to your repository. 

The queries are

## Step 1 - Count tweets [10 points]
Write a query in influxDB to count the number of tweets.

## Step 2 - Count the number of tweets of users who have no friends [10 points]
Write a query in influxDB for above.

## Step 3 - Show counts of tweet by Non American user called Jason [10 points]
Write a query in influxDB for above.


## Step-7 Timing Plots - 20 points

Read about timeit function call at https://docs.python.org/2/library/timeit.html

Write a function that run all your queries 10 times and produces a box plot per query. Read about https://pandas.pydata.org/pandas-docs/stable/reference/api/pandas.DataFrame.boxplot.html



## Grading Rubrics
* Queries - 30 points 
* Timing plots - 20 Points

























