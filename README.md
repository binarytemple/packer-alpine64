
# Background tips 

* [Creating a vagrant base box](https://www.vagrantup.com/docs/boxes/base.html)
* [Vagrant Cloud Post-Processor](https://www.packer.io/docs/post-processors/vagrant-cloud.html)

# Building 

Generate an Authentication token at https://app.vagrantup.com/settings/security

```
export ALPINE_VAGRANT_CLOUD_TOKEN=<Authentication token> 
export ALPINE_VAGRANT_IMAGE_NAME=bryanhuntesl/alpine64
packer build -force template.json
```

# Finding on Vagrant Cloud 

https://app.vagrantup.com/bryanhuntesl/boxes/alpine64

# Running built artefact 

Create a Vagrantfile like so:

```
Vagrant.configure("2") do |config|
  config.vm.box = "bryanhuntesl/alpine64"
  config.vm.box_version = "3.8.2"
  config.vm.synced_folder ".", "/vagrant", disabled: true

  config.ssh.shell = "/bin/ash"
  ssh_port = Random.new.rand(1000...5000)

  config.vm.network "private_network", ip: "33.33.33.2", type: "dhcp", auto_config: true
  config.vm.network "forwarded_port", guest: 22, host: "#{ssh_port}", id: 'ssh', auto_correct: true

  config.vm.provision "shell", inline: <<-SHELL
    echo "hello world"
  SHELL
end
```

Then run the following commands: 

```
vagrant plugin install vagrant-alpine
vagrant up
vagrant ssh
```

