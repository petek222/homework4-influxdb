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
	

Then execute the test_connection_and_load_tweets.ipynb
in colab  to load the nashville tweets database into AWS influxDB:

	


Download the lahman database to your windows or Mac Host  from http://www.seanlahman.com/files/database/
Use the lahman_sql_2012 comma delimited version (CSV) files. 

	$mkdir rawfiles
	$cd rawfiles
	$wget http://www.seanlahman.com/files/database/lahman2012-csv.zip
	$unzip lahman2012-csv.zip

**Note** - you might be asked to install unzip - follow prompts

if everything went well - it will look like following

```
~/rawfiles$ ls 
 AllstarFull.csv      AwardsPlayers.csv         Batting.csv       FieldingOF.csv     Managers.csv       Pitching.csv       Salaries.csv         SeriesPost.csv        TeamsHalf.csv
 Appearances.csv      AwardsShareManagers.csv   BattingPost.csv   FieldingPost.csv   ManagersHalf.csv   PitchingPost.csv   Schools.csv          Teams.csv
 AwardsManagers.csv   AwardsSharePlayers.csv    Fielding.csv      HallOfFame.csv     Master.csv        'readme 2012.txt'   SchoolsPlayers.csv   TeamsFranchises.csv
```


Then import the csv files into mongoDB using the below command.
Do this for all the .csv files

	$mongoimport -d <dbname> -c <collection_name>t --type csv --file <input.csv> --headerline.

Below are the import commands for all csv files to import into the mongodb - **you need to update the username and password to what you set up -- see the instructions above**

**Using an online "find-and-replace" tool to change the "username" and "yourpassword" fields for all below queries will make this process faster.**

	$mongoimport -d lahman -c AllstarFull --type csv --file AllstarFull.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c AwardsSharePlayers --type csv --file AwardsSharePlayers.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c Appearances --type csv --file Appearances.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c AwardsManagers --type csv --file AwardsManagers.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c AwardsShareManagers --type csv --file AwardsShareManagers.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c AwardsPlayers --type csv --file AwardsPlayers.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c Batting --type csv --file Batting.csv --headerline --username "ubuntu" --password "yourpassword"
 	$ls 
 	$mongoimport -d lahman -c Fielding --type csv --file Fielding.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c FieldingOF --type csv --file FieldingOF.csv --headerline --username "ubuntu" --password "yourpassword"
  	$mongoimport -d lahman -c FieldingPost --type csv --file FieldingPost.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c HallOfFame --type csv --file HallOfFame.csv --headerline --username "ubuntu" --password "yourpassword"
  	$mongoimport -d lahman -c Managers --type csv --file Managers.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c ManagersHalf --type csv --file ManagersHalf.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c Master --type csv --file Master.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c Pitching --type csv --file Pitching.csv --headerline --username "ubuntu" --password "yourpassword"
  	$mongoimport -d lahman -c PitchingPost --type csv --file PitchingPost.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c Salaries --type csv --file Salaries.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c Schools --type csv --file Schools.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c SchoolsPlayers --type csv --file SchoolsPlayers.csv --headerline --username "ubuntu" --password "yourpassword"
  	$mongoimport -d lahman -c SeriesPost --type csv --file SeriesPost.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c Teams --type csv --file Teams.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c TeamsFranchises --type csv --file TeamsFranchises.csv --headerline --username "ubuntu" --password "yourpassword"
 	$mongoimport -d lahman -c TeamsHalf --type csv --file TeamsHalf.csv --headerline --username "ubuntu" --password "yourpassword"


**you can use a cool shell command to import all**

	$for file in `ls *.csv`; do mongoimport -d lahman -c `basename $file .csv` --type csv $file --headerline --username "ubuntu" --password "yourpassword";done



## Step-5 Check initial Colab Connection

Run the Colab connection script [test-setup.ipynb](test-setup.ipynb) and ensure that you get the connection and the number of tables correctly. Make sure that you update the database name, the username and the password. 

**Note** the port should be 27017 and the host should be the hostname of your AWS instance.

Remember to shutoff the EC2 instance when you are not using it.

At this point check initial connection from compass as well. During connection choose lahman as the authentication database. And provide the username and password you created for lahman database.

 If you opened the ports correctly the connection will work and you can get something like following


![](images/compass.png)

![](images/compass2.png)

![](images/compass3.png)

## Step-6 Queries - 80 points- 

Implement a function per query in a file called [hw3.ipynb](hw3.ipynb). Record the answers there and save it back to your repository. 

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

Also look at the weather box plot example in traffic example notebook in this repository




## Grading Rubrics
* Queries - 80 points 
* Timing plots - 20 Points

























