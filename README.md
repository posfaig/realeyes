## Introduction

Imaging the following conditions:

### Infrastructure
* Windows, MacOS or Linux local workstations per each user
* Two powerful local servers (OS: Ubuntu 14.04)
* Different Amazon EC2 instances launched on demand
* MSSQL Server as Operational Database (Amazon RDS)
* AWS Stack (S3, RDS, DynamoDB, etc)
		
#### Limitations

* Limited bandwidth channel (shared) to workstations and local servers
* Used data does not fit into servers RAM
* Many libraries and packages on local servers are outdated and cannot be updated, because of some other specific software
	
### Sample data

* **Adhoc** - various data, mostly in csv format, results of some preprocessing or provided by customers
* **Database** - Operational database
* **Training** - training database files in parquet format
Real size of the data: several terabytes.

### Example daily usage
To classify emotions:

* Load training subset of data from the training database (frames)
* Optionally join it with the data from the operational database
* Train your emotion classifier
* Validate it using the test split from training database and store results of the validation for comparison

To train classifier based on emotion output:

* Load subset of the data from operational database (labels)
* Load ground truth data from adhoc data
* Train classifier
* Validate it using cross-validation and store results back to operational database

Obtaining statistics:

* Query operational database
* Query training database
* Load adhoc data
* Combine results in some way

## Problems
Based on the provided use-cases, it can be seen that there are several main problems: 

* To get some statistics you need to load data from different sources and lately somehow combine it
* To train a classifier based on emotions output you need to query operational database and it could block other queries, in addition, usually it is combined lately with some *adhoc* data 
* On average emotion output data is relatively big (up to hundred gigabytes), so loading it from operational database to local servers could take some significant amount of time

## Main goal
So, the main goal is to design and implement PoC for the Data Handling Platform which could solve defined above problems, based on the described infrastructure and limitations, provided sample data in ***data.zip*** archive and usage information.

## Tasks

* Create a docker image with Operational Database (any database engine could be used) and write scripts to populate it from provided csv files (in data.zip/database) - each csv file should become a separate table
* Describe and draw a high-level architecture diagram of the Data Handling Platform
* Create Data Handling Platform proof of concept in a form of a docker image; or alternatively, in case of IaaS, PaaS, SaaS solution - create a sample test account and provide credentials to it
* Code a simple ETL pipeline to the Data Handling Platform from **Operational Database** (created in the first task), **adhoc** (in data.zip/adhoc) and **training data** (in data.zip/training) as input sources
* Implement a python client package (preferably integrated with Python Pandas Dataframes) for the platform and write a small document how to use it
* Prepare your homework bundle in such a way that it could be easily run on MacOS or Linux

## Notes
Please send your homework even if you did not complete all the tasks in allocated time