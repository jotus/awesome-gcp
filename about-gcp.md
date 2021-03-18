

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
