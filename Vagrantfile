Vagrant.configure("2") do |config|
  config.ssh.insert_key = false
  config.vm.synced_folder ".", "/vagrant", disabled: true
 
IMAGEN = "geerlingguy/ubuntu2004"

  config.vm.define "darkstat" do |t|
    t.vm.box = IMAGEN
    t.vm.hostname = "darkstat"
    t.vm.network "public_network", bridge: "enp1s0"
    t.vm.network "forwarded_port", guest: 666, host: 8666
    
    t.vm.provision "shell", inline: <<-SHELL 
    sudo apt-get update -qq && sudo apt-get install -y darkstat
    sudo sed -i -e 's*START_DARKSTAT=no*START_DARKSTAT=yes*' -e 's*INTERFACE=\"-i eth0\"*INTERFACE=\"-i enp0s8\"*' -e 's*#DIR=\"\/var\/lib\/darkstat\"*DIR=\"\/var\/lib\/darkstat\"*' -e 's*#PORT=\"-p 666\"*PORT=\"-p 666\"*' /etc/darkstat/init.cfg
    sudo systemctl start darkstat
    sudo /lib/systemd/systemd-sysv-install enable darkstat
    sudo systemctl restart darkstat
    SHELL
  end  
  
  config.vm.provider "virtualbox" do |vb|
    vb.name = "darkstat"
    vb.memory = "1024"
    vb.cpus = "2"
  end  
end


