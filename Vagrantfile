Vagrant.configure("2") do |config|
    config.vm.box = "debian/contrib-buster64"
    config.vm.hostname = "boxey"
    config.disksize.size = '50GB'

    # Because this is a local development machine that does is only accessible
    # locally on the machine it is running from, forwarding ports liberally
    # is for the time being an acceptable security risk. 

    config.vm.network "forwarded_port", guest: 8000, host: 8080, host_ip: "127.0.0.1"

    config.vm.provider "virtualbox" do |v|
      v.memory = 6000
    end
  
    # Share my projects directory with the VM
    config.vm.synced_folder "../", "/home/vagrant/projects"
    config.vm.synced_folder "./", "/boxey"
 
    # Tell Vagrant not to insert its own SSH key, we will use our own.
    config.ssh.insert_key = false 

    # Copy over the host private key in order to push to git
    config.vm.provision "file", source: "~/.ssh/id_rsa", destination: "~/.ssh/id_rsa"

    # Provision the Vagrant box with Ansible. 
    config.vm.provision "ansible_local" do |ansible|
        #ansible.verbose = "v"
        ansible.playbook = "/boxey/main.yml"
    end
end
