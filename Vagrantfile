Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.define "db" do |db|
    db.vm.box = "ubuntu/trusty64"
    db.vm.network :private_network, ip: "192.168.50.50"
    db.vm.provider "virtualbox" do |v|
        v.memory = 512
    end
    db.vm.provision "ansible" do |ansible|
        ansible.playbook = "provisioning/playbook.yml"
        #ansible.verbose = 'v'
    ansible.extra_vars = {
       db: {
            name: "app"
       },
       dbuser: {
            name: "worker",
            password: "worker"

       }
     }
    end
  end

  config.vm.define "app" do |app|
    app.vm.box = "ubuntu/trusty64"
    app.vm.network "forwarded_port", guest: 8080, host: 8080
    app.vm.synced_folder "./", "/vagrant"
    app.vm.network :private_network, ip: "192.168.50.51" 
    app.vm.provider "virtualbox" do |v|
        v.memory = 512    
    end    
    app.vm.provision "ansible" do |ansible|
        ansible.playbook = "provisioning/playbook.yml"
        #ansible.verbose = 'vvv'
    ansible.extra_vars = {
       user: {
            name: "www-data",
            group: "www-data",
            home: "/var/www",
            shell: "/bin/bash"
       },
       pyenv: {
            local_pull: "no",
            version: "3.6.5"
        },
       db: {
            name: "app"
       },
       dbuser: {
            name: "worker",
            password: "worker"

       }
     }
    end
  end
end
