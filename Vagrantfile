# -*- mode: ruby -*-
# vi: set ft=ruby :

Vagrant.configure("2") do |config|

  # vagrant-vbguest plugin. See https://github.com/dotless-de/vagrant-vbguest for more details
  # set auto_update to false, if you do NOT want to check the correct additions version when booting this machine
  config.vbguest.auto_update = false
  # do NOT download the iso file from a webserver
  config.vbguest.no_remote = true
  
  config.vm.define "dev1" do |dev1|
    dev1.vm.box = "ubuntu/trusty64"
    dev1.vm.box_check_update = false
    dev1.vm.network "private_network", ip: "192.168.33.10"
    dev1.vm.synced_folder ".", "/vagrant", disabled: true
    dev1.vm.synced_folder "vagrant/", "/vagrant/shared/"
    # Provider-specific configuration
    dev1.vm.provider "virtualbox" do |vb|
      # vb.gui = true
      vb.name   = "cd-env-dev1"
      vb.memory = "2048"
      vb.cpus   = 1
    end
    # Enable provisioning with a shell script
    dev1.vm.provision "shell", inline: "echo *** Provisioning dev1! ***"
    # Enable provisioning with docker
    dev1.vm.provision "docker" do |d|
      
      # see https://hub.docker.com/_/httpd/
      # d.pull_images "httpd:2.4.29"
      # d.run "httpd", image: "httpd:2.4.29", args: "-d -p 80:80"
      
      # see https://hub.docker.com/r/atlassian/bitbucket-server/
      d.pull_images "atlassian/bitbucket-server:5.6.2"
      d.post_install_provision "shell", inline:"docker volume create --name bitbucketVolume"
      d.run "bitbucket", image: "atlassian/bitbucket-server:5.6.2", args: "-d -p 7990:7990 -p 7999:7999 -v 'bitbucketVolume:/var/atlassian/application-data/bitbucket'"
      
      # see https://hub.docker.com/r/sonatype/nexus3/
      d.pull_images "sonatype/nexus3:3.7.0"
      d.post_install_provision "shell", inline:"docker volume create --name nexusVolume"
      d.run "nexus", image: "sonatype/nexus3:3.7.0", args: "-d -p 8081:8081 -v 'nexusVolume:/nexus-data'"
    end
  end
  
  config.vm.define "dev2" do |dev2|
    dev2.vm.box = "ubuntu/trusty64"
    dev2.vm.box_check_update = false
    dev2.vm.network "private_network", ip: "192.168.33.11"
    dev2.vm.synced_folder ".", "/vagrant", disabled: true
    dev2.vm.synced_folder "vagrant/", "/vagrant/shared/"
    # Provider-specific configuration
    dev2.vm.provider "virtualbox" do |vb|
      # vb.gui = true
      vb.name   = "cd-env-dev2"
      vb.memory = "2048"
      vb.cpus   = 1
    end
    # Enable provisioning with a shell script
    dev2.vm.provision "shell", inline: "echo *** Provisioning dev2! ***"
    # Enable provisioning with docker
    dev2.vm.provision "docker" do |d|
    
      # see https://hub.docker.com/r/jenkins/jenkins/
      d.pull_images "jenkins/jenkins:2.89.2"
      d.post_install_provision "shell", inline:"docker volume create --name jenkinsVolume"
      d.run "jenkins", image: "jenkins/jenkins:2.89.2", args: "-d -p 8080:8080 -p 50000:50000 -v 'jenkinsVolume:/var/jenkins_home'"
    end
  end

end
