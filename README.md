

#  Choice for this project was Grafana

Chose because I was recommended and it sounded interesting
## what is Grafana? 

Grafana, by definition, is a program used for visualization, to understand and watch data, it is open source.

Grafana itself doesnt store data, it visualizes it

It can connect to a large amount of sources of data, some examples are Graphite, Influxdb and many more

It possesses a large amount of tools to display the data, from simple graphs, to more complex things like heat maps

Unified dashboaring, you can use it to combine different data, from multiple sources, to one dashboard, an example of this is, you could show cpu and gpu usage, and error logs all on the same dashboard to find out errors and where they come from

Grafana can also be used for alerts, for example if cpu usage reaches a certain amount, it could email you that it reached that threshold
#  Suitable instance type
aws t.3 medium / gcp e2-medium  2 cpus and 4 gbs of ram handles several users perfectly

## Storage
20 gbs of storage in an ssd would be preferable, but a hard disk drive can also work
# 1 OS chosen

Ubuntu is going to be what I chose, as its both good for the job and the OS I have the most experience with
24.04 is ubuntu version
Linux is also the native habitat for Grafana so that simplifies the choice significantly 

# 2 Installation Steps

## First
sudo apt-get install -y adduser libfontconfig1 musl
## Second
wget https://dl.grafana.com/grafana-enterprise/release/13.0.1/grafana-enterprise_13.0.1_24542347077_linux_amd64.deb
## Third
sudo dpkg -i grafana-enterprise_13.0.1_24542347077_linux_amd64.deb

after this, if admin use the ip given with the key pair to access grafana
## follow the above listed commands
## After this, the terminal should tell you grafana was installed correctly

# 3 Access Plan

## Server Administration
Method SSH via public keys
use this for OS updates, plugin manual installs, and configuration files
## User Access for human users
Role based controls RWE (Read Write Edit)

## System Access
Service Accounts, instead of just using API keys, I am gonna use service accounts as they are easier (this may change cause API is also easier to setup)

# 4 Demonstration 
## this will be done once done 

# 4 part 2 cost estimates 

## estimates rougly 

30$ / month for an instance running all the time 

EIP is 0 a month cause its attached to a running aws instance

AMI 0 a month, free on ubuntu

35.00$ a month expected, which includes all aspects, including storage
# 5 AWS setup and instructions 

## VPC setup
| Component | Configuration | Justification |
| :--- | :---: | ---: |
| VPC block | 10.0.0.0/16  | Standart private range that allows 65536 IPS, offers rooms to scale later as well |
| Subnet Block | 10.0.1.0/24 | keeping it in /24 makes it easier small scale,this is used for the grafana front end |
| Route Table | 0.0.0.0/0 IGW | This is needed to be able to fetch updates, and be able to reach the internet |
| Network ACL | Allow inboud, 443, 22 | A secondary defense for the subnet itself |

## Security Group rules

| Protocol | Port | Source | Purpose |
| :--- | :---: | ---: | ---: |
| SSH | 22 | Only Admin IP | Secure Remote Admin Access |
| HTTPS | 443 | 0.0.0.0/0 | End User Access | 
| Custom TCP | 3000 | Admin IP | initial admin setup |
# 6 Security and Access
## Admin
Admininstration will be done via ssh by using RSA keys
## Application Access
Users will use it via a browser
Admin has full control
User can only view
## System based firewall
ufw is enabled on OS level

# 7 Software Features
Data source integration, seamless connection is useful for this

provisioning,using YAMl files to automatically load and use dashboards

it also alerts automatically

# 8 Backup and Disaster using the 3-2-1 rule

3 copies, in Live DB, local and S3 cloud  this ensures that there are three points of failure, ensuring a higher degree of overall safety and keeping the safety of having three failure points instead of one

2 media, SSD locally, and object storage in S3, this ensures that there is a backup if there is an ssd failure, and that the ssd failing doesnt destroy and harm the program

1 Offsite, S3 bucket in a different AWS region, which creates a backup every about 2 hourss, to ensure as little loss as possible, this will ensure that even if the onsite is lost, you will not lose  all of your progress 
This also projects against regional outages, natural disastors, and infastructure failure
roughlu 500 MB will be used roughly

15 minute estimated recovery time

24 hour recovery point

actual goal is less than 30 minutes for the recovery, but its expected to be around 15 minutes
# 9 common trouble shooting

## Plugin Signature Verification
if a plugin wont load, its likely because grafana.ini hasnt been updated and doesnt allow unsigned plugins if using community tools

## Data source timeout

This is usually a issue caused by the grafana security not having permission to reach the data source

# Extra Stuff, access plan, etc

## Access plan and Seucirty

Make the Server admin be accessible through only ssh through port 22

Users access through port 443

Systems, external apps connect using service accounts 

## AWS specifics


### instance specifics
 I chose for my instance, ubuntu as thats the OS I was using, giving it the 20 gb of storage, using the grafana vpc, grafana security group and a key pair for grafana 

### network ACL specifics

for my network acl rules, I allowed all traffic, as I sued my security group to deny who I want to, so throug the acl I allowed all rules
It also has an association with one subnet, which is the grafana one

### Route table specifics
for route table, two routes, 0.0.0.0/0  to my internet gateway to create the table and to control it

10.0.0.0/16 for local to allow me local access

### Subnet specifics
associated with all the grafana stuff mentioned previously
249 available ipv4 addressses 
this is to make sure the rest works

### VPC specifics
set to 10.0.0.0/16 with all the grafana things on it, such as security group, route table, etc 
this allows me to put it on a instance and run the ssh
I wanted it public because public can only read, not write or execute
### Security specifics

I have 4 rules in my security group

#### HTTP port 80
 used to redirect users for ssl verification
#### Custom TCP port 3000
this is used to test it and to run stuff initially, I may delete this once I dont need it
#### HTTPS ports 443 
this is used to allow users access to the grafana terminal
#### SSH port 22
used for admin stuff, port 22 for ssh into it

#### storage amount and reason
I set 20 gb so I had plenty to use it properly`
I also wanted plenty of space so I didnt go over, 20 gb seemed appropriate for this

# Screenshot 
<img width="1919" height="968" alt="Screenshot 2026-04-29 104530" src="https://github.com/user-attachments/assets/773eb5b5-7e50-4acf-9ec7-80d10c8f9b0b" />
Screenshot of Grafana Running
<img width="1241" height="673" alt="Screenshot 2026-04-29 105651" src="https://github.com/user-attachments/assets/1bde95fa-4d52-410f-9056-6b9136bca5f9" />
Grafana running off an invalid IP (proof of functioning firewall)
<img width="1538" height="876" alt="Screenshot 2026-04-29 110640" src="https://github.com/user-attachments/assets/cf3cba9f-bca8-4463-9eb5-711886d0521f" />
demonstration of the begining of adding into grafna
<img width="1597" height="274" alt="Screenshot 2026-04-29 111249" src="https://github.com/user-attachments/assets/ee9cd909-aa94-4225-bb9d-811e657a7ec4" />
instance
<img width="1610" height="369" alt="Screenshot 2026-04-29 111306" src="https://github.com/user-attachments/assets/f172e9bf-8384-4a0d-b38b-f69df2ed00c8" />
grafana security group
<img width="1578" height="210" alt="Screenshot 2026-04-29 111317" src="https://github.com/user-attachments/assets/cce91944-264a-4062-bc44-44b34420692e" />
vpc for grafana
<img width="1615" height="377" alt="Screenshot 2026-04-29 111344" src="https://github.com/user-attachments/assets/095361d4-5664-430a-9dca-f8108561a03e" />
subnet for grafana
<img width="1618" height="379" alt="Screenshot 2026-04-29 111352" src="https://github.com/user-attachments/assets/963b10cd-5baf-4af9-9c58-4d7ec1698f02" />
<img width="1580" height="357" alt="Screenshot 2026-04-29 111603" src="https://github.com/user-attachments/assets/5c47c698-bca8-465c-be31-a7574ae7a061" />
<img width="1629" height="321" alt="Screenshot 2026-04-29 111609" src="https://github.com/user-attachments/assets/653efa69-de44-4e8e-8f03-498e5ff7941a" />
route table screenshots for grafana
<img width="1608" height="273" alt="Screenshot 2026-04-29 111407" src="https://github.com/user-attachments/assets/be50184d-a778-404e-9d66-691b0a8f5d2d" />
internet gateway for grafana
<img width="1905" height="942" alt="Screenshot 2026-04-29 112718" src="https://github.com/user-attachments/assets/2be3e072-6506-43a5-8796-c5336efd6219" />
access as user with no write access
this is the read only user
<img width="1907" height="937" alt="Screenshot 2026-04-29 113713" src="https://github.com/user-attachments/assets/0ac6e994-80b8-457a-a599-7871bed1d90b" />
what it does when ssh link is closed








