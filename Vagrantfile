# -*- mode: ruby -*-
# vi: set ft=ruby :
# Example Docker edited by Jessica Rankins 6/15/2017

#Docker automatically puts containers on the bridge network
#****************************************
#
# Must run "vagrant docker-exec WebContainer -- sudo service apache2 restart"
#    for apache server to work and 
#	 "vagrant docker-exec DBContainer -- sudo service mysql restart"
#	 for mysql to work
#
#****************************************

HOST_SYNC_FOLDER = Dir.pwd + "/public-html"
CONTAINER_SYNC_FOLDER = "/var/www/html"

VOLUME = "//" + HOST_SYNC_FOLDER[0].downcase + "//" + HOST_SYNC_FOLDER[3..-1] + ":" + "#{CONTAINER_SYNC_FOLDER}"


  # All Vagrant configuration is done below. The "2" in Vagrant.configure
  # is the configuration version.
Vagrant.configure("2") do |config|
  
  #config.vm.network "forwarded_port", guest: 8080, host: 8080, id: "Web"
  # Have to do this by going to VirtualBox, Settings of default boot2docker
    # container, Network, NAT Advanced, Port Forwarding, create new with:
	# id - Web, Host IP - 127.0.0.1, Host Port - 8080, Guest Port - 8080
  # OR.. 'VBoxManage controlvm "boot2docker-vm" natpf1 "tcp-port8000,tcp,,8000,,8000"'
	# after vagrant up (But I do not have VBoxManage...)
	
  # Read only file system on boot2docker. Instead create volume
  config.vm.synced_folder Dir.pwd, "/vagrant", disabled: true
  
    
  # Configure the Web Server
  config.vm.define "WebContainer" do |web|
    web.vm.provider :docker do |d|
      # Can specify an image or a Dockerfile: 
      d.build_dir = "."
	  d.dockerfile = "WebDockerfile"
      d.build_args = [ "-t", "mywebimage" ]
      d.create_args = [ "-i", "-t", "--network=my-bridge-network", "--ip=172.18.0.2" ]
      d.has_ssh = false
      d.ports = [ "8080:80" ]
      d.name = "webcontainer"
	  d.volumes = [ "#{VOLUME}" ]
	end
    # After vagrant up, should see VM's web page in browser at 127.0.0.1:8080
  end    # End WebContainer config


  # Configure the Database
  config.vm.define "DBContainer" do |db|
    db.vm.provider :docker do |d|
      # Can specify an image or a Dockerfile: 
      d.build_dir = "."
	  d.dockerfile = "DBDockerfile"
      d.build_args = [ "-t", "mydbimage" ]
      d.create_args = [ "-i", "-t", "--network=my-bridge-network", "--ip=172.18.0.3" ]
	  d.ports = [ "6603:3306" ]
      d.has_ssh = false
      d.name = "dbcontainer"
    end
  end    # End DBContainer config

end    # End configuring