
Vagrant.configure("2") do |config|

  config.vm.define "local" do |local|
    # local.vm.box = "centos/7"
    local.vm.box = "ubuntu/bionic64"
    # local.vm.box = "bento/ubuntu-16.04"
    local.ssh.insert_key = false
    local.vm.hostname = "mediawiki.box"
    local.vm.synced_folder ".", "/vagrant", create: true, disabled: false
    local.vm.network :private_network, ip: "192.168.98.121"

    local.vm.provider :virtualbox do |vb|
      vb.name = "mediawiki"
      vb.memory = 2048
      vb.cpus = 2
    end
  end

  config.vm.provision "ansible_local" do |ansible|
    ansible.install = true
    ansible.install_mode = "pip"
    # Vagrant 2.2.5: fix pip installation by uncommenting the next line (see https://github.com/hashicorp/vagrant/issues/10950)
    #ansible.pip_install_cmd = "curl https://bootstrap.pypa.io/get-pip.py | sudo python"
    ansible.version = "latest"
    ansible.compatibility_mode = "2.0"
    ansible.playbook = "ansible/system.yml"
    ansible.groups = {
      "mediawiki" => ["local"],
    }
    # comment/uncomment next line to activate/deavtivate pre-installation of edu-sharing-plugin (=> has to be finished manually, see README)
    ansible.skip_tags = [ "edu-sharing-plugin" ]
  end
end
