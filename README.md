###  Choice for this project was Grafana
Chose because I was recommended and it sounded interesting

##  Suitable instance type
aws t.3 medium / gcp e2-medium  2 cpus and 4 gbs of ram handles several users perfectly

# Storage
20 gbs of storage in an ssd would be preferable, but a hard disk drive can also work
## 1 OS chosen

Ubuntu is going to be what I chose, as its both good for the job and the OS I have the most experience with
24.04 is ubuntu version
Linux is also the native habitat for Grafana so that simplifies the choice significantly 

## 2 Installation Steps

# First
sudo apt-get install -y adduser libfontconfig1 musl
# Second
wget https://dl.grafana.com/grafana-enterprise/release/13.0.1/grafana-enterprise_13.0.1_24542347077_linux_amd64.deb
# Third
sudo dpkg -i grafana-enterprise_13.0.1_24542347077_linux_amd64.deb

# follow the above listed commands
# After this, the terminal should tell you grafana was installed correctly

## 3 Access Plan

# Server Administration
Method SSH via public keys
use this for OS updates, plugin manual installs, and configuration files
# User Access for human users
Role based controls RWE (Read Write Edit)

# System Access
Service Accounts, instead of just using API keys, I am gonna use service accounts as they are easier (this may change cause API is also easier to setup)

##4 Demonstration 
#this will be done once done 


##5 AWS setup and instructions 
#this is done out of order as im doing it when I actually did it

## 6 Security and Access
#Admin
Admininstration will be done via ssh by using RSA keys
# Application Access
Users will use it via a browser
Admin has full control
User can only view
#System based firewall
ufw is enabled on OS level

##7 Software Features
Data source integration, seamless connection is useful for this

provisioning,using YAMl files to automatically load and use dashboards

it also alerts automatically

##8 Backup and Disaster using the 3-2-1 rule

3 copies, in Live DB, local and S3 cloud

2 media, SSD locally, and object storage in S3

1 Offsite, S3 bucket in a different AWS region

roughlu 500 MB will be used roughly

15 minute estimated recovery time

24 hour recovery point

## 9 common trouble shooting

# Plugin Signature Verification
if a plugin wont load, its likely because grafana.ini hasnt been updated and doesnt allow unsigned plugins if using community tools

# Data source timeout

This is usually a issue caused by the grafana security not having permission to reach the data source
