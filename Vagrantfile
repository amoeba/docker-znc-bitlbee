# -*- mode: ruby -*-
# vi: set ft=ruby :

BOX_NAME = ENV['BOX_NAME'] || "ubuntu"
BOX_URI = ENV['BOX_URI'] || "http://files.vagrantup.com/precise64.box"
VF_BOX_URI = ENV['BOX_URI'] || "http://files.vagrantup.com/precise64_vmware_fusion.box"
AWS_BOX_URI = ENV['BOX_URI'] || "https://github.com/mitchellh/vagrant-aws/raw/master/dummy.box"
AWS_REGION = ENV['AWS_REGION'] || "us-east-1"
AWS_AMI = ENV['AWS_AMI'] || "ami-69f5a900"
AWS_INSTANCE_TYPE = ENV['AWS_INSTANCE_TYPE'] || 't1.micro'
AWS_SECURITY_GROUP = ENV['AWS_SECURITY_GROUP']
AWS_SSH_PRIVKEY_PATH = ENV['AWS_SSH_PRIVKEY_PATH']

FORWARD_DOCKER_PORTS = ENV['FORWARD_DOCKER_PORTS']

SSH_PRIVKEY_PATH = ENV["SSH_PRIVKEY_PATH"]

Vagrant.configure("2") do |config|
  # Setup virtual machine box. This VM configuration code is always executed.
  config.vm.box = BOX_NAME
  config.vm.box_url = BOX_URI

  config.ssh.forward_agent = true

  config.vm.provision :docker
  config.vm.provision :shell, :inline => <<END
service docker stop
sed -i 's/DOCKER_OPTS=/DOCKER_OPTS="-H 0.0.0.0:4243"/' /etc/init/docker.conf
service docker start
END

  config.vm.provider :aws do |aws, override|
    username = "ubuntu"
    override.vm.box_url = AWS_BOX_URI
    aws.access_key_id = ENV["AWS_ACCESS_KEY"]
    aws.secret_access_key = ENV["AWS_SECRET_KEY"]
    aws.keypair_name = ENV["AWS_KEYPAIR_NAME"]
    override.ssh.username = username
    aws.region = AWS_REGION
    aws.ami    = AWS_AMI
    aws.instance_type = AWS_INSTANCE_TYPE
    if AWS_SECURITY_GROUP
      aws.security_groups = [AWS_SECURITY_GROUP]
    end
    if AWS_SSH_PRIVKEY_PATH
      aws.ssh.private_key_path = AWS_SSH_PRIVKEY_PATH
    end
  end

  config.vm.provider :virtualbox do |vb, override|
    vb.customize ["modifyvm", :id, "--natdnshostresolver1", "on"]
    vb.customize ["modifyvm", :id, "--natdnsproxy1", "on"]
    override.vm.network :forwarded_port, :host => 4243, :guest => 4243
    override.vm.network :forwarded_port, :host => 6668, :guest => 6668
  end
end
