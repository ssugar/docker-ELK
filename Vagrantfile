# -*- mode: ruby -*-
# vi: set ft=ruby :

# Vagrantfile API/syntax version. Don't touch unless you know what you're doing!
VAGRANTFILE_API_VERSION = "2"

$prereboot = <<SCRIPT
sudo su
echo "deb http://mirror-fpt-telecom.fpt.net/ubuntu/ precise main restricted universe" > /etc/apt/sources.list
echo "deb http://mirror-fpt-telecom.fpt.net/ubuntu/ precise-updates main restricted universe" >> /etc/apt/sources.list
echo "deb http://mirror-fpt-telecom.fpt.net/ubuntu/ precise-security main restricted universe" >> /etc/apt/sources.list
echo "Asia/Ho_Chi_Minh" > /etc/timezone
dpkg-reconfigure --frontend noninteractive tzdata
apt-get update
apt-get install curl -y
apt-get install nano -y
apt-get install linux-image-generic-lts-raring linux-headers-generic-lts-raring -y
SCRIPT

$postreboot = <<SCRIPT
sudo apt-get install apparmor -y
curl -sSL https://get.docker.com/ubuntu/ | sudo sh
sudo su
docker run -d -p 9200:9200 -p 9300:9300 dockerfile/elasticsearch
docker run -d -e LOGSTASH_CONFIG_URL=https://raw.githubusercontent.com/ssugar/netmon-ELK/master/cookbooks/ss_logstash/files/default/logstash.conf -p 9292:9292 pblittle/docker-logstash
SCRIPT


Vagrant.configure(VAGRANTFILE_API_VERSION) do |config|
   #Set the virtual machine 'box' to use
   config.vm.box = "hashicorp/precise64"
   #Set the vm name
   config.vm.define :dockerELK do |t|
   end
   
   #run the pre-reboot script
   config.vm.provision "shell", inline: $prereboot

   #reboot
   config.vm.provision :reload
   
   #run the script above
   config.vm.provision "shell", inline: $postreboot

end
