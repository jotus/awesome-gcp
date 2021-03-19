

# Intro

# region, zones, edge locations
Zones are physical location

Region-1
Zone-1, Zone-2, Zone-3

eg. US-Central1
US-Central1A, US-Central1B, US-Central1C


Security
Google provides fiber network, dont need to touch internet.

Edge locations
User request are responded from Edge location , lowest latency.
Edge location contacts nearest google datacenter

# Why google
- google security model
- private fiber global network
 23 regions, 17 zones, 140 edge location
 - Live migration
 - dedicated machine learnign and BigData

 # Project overview
 Go to project
  --> LIbrary
   > Compute Engine API
   > Cloud SQL

# IAM - identity and access management
Provides granular access control

Go to IAM
roles: editor , owner, viewer

## ORganization resoure
Is root resource for other resources in gcp resource hierarchy.
Serves as hierarchical ancestor for Folders and Projects.
Folder:
Can serve as different teams or  deps withing company.
Helps allocate billing for the resources.

## GC Projects
What it google cloud project?
It organizes resources, consists of set of users, a set of API's and billing,
authentication and monitoring settings for those API.

Create Project
Look at activity screan
eg. my project 1

# VPC networking
VPC is virtual version of physical network implemented inside google production network.
VPC Routing 
Global resource not associated with any region/zone

In region there is subnet configured
Project:
  VCP Routing:
    - Region-1:
        -- subnet-1:
          -- Zone-1
            -- VM1
            -- VM2
          -- Zone-2
        -- subnet-2:
    - Region-2:
        -- subnet-1:
            -- Zone-1
            -- Zone-2
        -- subnet-2:

### Exercise
 - go to VPC Networks
 - crete vpc 
    -- address range: 10.0.0.0/16

# Compute engine
Lets run vm on google infra
When creating Vm you specify service account thats bound to application rather than user

## Machine type
Set of virtualized hardware resources available to virtual machine instance including
system memory size, virtual CPU count, persistent disk limits.

 Types
 - E  - narmal, used for day to day computing, cost efficient
 - M - memory optimized
 - N - general purpose
 - C - compute optimized

 # Cloud storage like S3
Globally unified scalable and highly durable object storage for developers and enterprises.
Provides StorageCloud for any Workload

## Storage classes
- Standard 
Frequently accessed data
- nearline
low cost higly durable storage , frequently accessed data
- coldline 
similar to nearline, but when you acces data less frequently eg. once a quarter
- archive
lowest cost, backup, disaster ricover, accessed rarely

## Exercices
- go to cloud storage
- create cloud bucket, should be globally unique,

# Cloud SQL 
It covers mysql, postgres, sql server engine
Cloud spanner - glabally distributed db with unlimited space

# NoSql
BigTable - for analytical data
Firestore -
MemoryStore - in memory service for memcached and redis

# BigQuery
Serverless datawarehouse designed for business agility.

# Google Dataflow
managed service to run ApacheBeam pipelines withing GCP ecosystem.
Uses PCollection - distributed collection 
It automatically partitinos and distributes data across vms

# PubSub
managed real time messaging  service allows to send and receive messages between independent applications.

# Monitoring resources in GCP
 - go to monitoring dashboard - may merge few projects

# Machine learning
Tensorflow
Opensource lib for ML