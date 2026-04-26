###  Choice for this project was Grafana
Chose because I was recommended and it sounded interesting

##  Suitable instance type
aws t.3 medium / gcp e2-medium  2 cpus and 4 gbs of ram handles several users perfectly

# Storage
20 gbs of storage in an ssd would be preferable, but a hard disk drive can also work
##  OS chosen

Ubuntu is going to be what I chose, as its both good for the job and the OS I have the most experience with
24.04 is ubuntu version
Linux is also the native habitat for Grafana so that simplifies the choice significantly 

## Installation Steps
sudo apt-get install -y apt-transport-https software-properties-common wget
sudo mkdir -p /etc/apt/keyrings/
wget -q -O - https://apt.grafana.com/gpg.key | gpg --dearmor | sudo tee /etc/apt/keyrings/grafana.gpg > /dev/null
echo "deb [signed-by=/etc/apt/keyrings/grafana.gpg] https://apt.grafana.com stable main" | sudo tee /etc/apt/apt/sources.list.d/grafana.list
sudo apt-get update && sudo apt-get install grafana
sudo systemctl enable --now grafana-server
# follow the above listed commands
