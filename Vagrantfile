
Vagrant.configure("2") do |config|

  config.vm.define "local" do |local|
    # local.vm.box = "centos/7"
    local.vm.box = "ubuntu/bionic64"
    # local.vm.box = "bento/ubuntu-16.04"
    local.ssh.insert_key = false
    local.vm.hostname = "mediawiki.box"
    local.vm.synced_folder ".", "/vagrant", create: true, disabled: false
    local.vm.network :private_network, ip: "192.168.98.101"

    local.vm.provider :virtualbox do |vb|
      vb.name = "mediawiki"
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.install = true
    ansible.install_mode = "pip"
    ansible.version = "latest"
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "ansible/system.yml"
  end
end
