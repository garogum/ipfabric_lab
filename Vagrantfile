# Resources:
# https://www.dravetech.com/blog/2016/01/08/vagrant-for-network-engineers.html
# http://www.fredrikholmberg.com/2016/04/on-demand-juniper-labs-using-vagrant/
# https://github.com/sciarrilli/arista-ansible/blob/master/Vagrantfile
# https://ripe73.ripe.net/presentations/3-Network-Automation-Tutorial.pdf

Vagrant.configure(2) do |config|

  config.vm.define "base1" do |base|
    base.vm.box = "ubuntu/xenial64"

    base.vm.network :forwarded_port, guest: 22, host: 12200, id: 'ssh'

    base.vm.network "private_network", virtualbox__intnet: "l1-base1",
                                       ip: "10.100.0.2"

    base.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install lldpd -y
# install stuff for pmacct
      sudo apt-get install libpcap-dev libsqlite3-dev libjansson-dev zlib1g-dev gcc make sqlite3 -y
   SHELL
  end

  config.vm.define "base2" do |base|
    base.vm.box = "ubuntu/xenial64"

    base.vm.network :forwarded_port, guest: 22, host: 12207, id: 'ssh'
    base.vm.network :forwarded_port, guest: 8000, host: 8123, id: 'sir'

    base.vm.network "private_network", virtualbox__intnet: "l3-base2",
                                       ip: "10.200.0.2"

    base.vm.provision "shell", inline: <<-SHELL
      sudo apt-get update
      sudo apt-get install lldpd -y
# install stuff for pmacoct
# https://sdn-internet-router-sir.readthedocs.io/en/latest/how_to_generic/pmacct.html
# export LC_ALL=C
      sudo apt-get install libpcap-dev libsqlite3-dev libjansson-dev zlib1g-dev gcc make sqlite3 -y
      sudo apt-get install python-pip -y
      sudo pip install SIR
      sudo pip install gunicorn
   SHELL
  end

  config.vm.define "leaf1" do |eos|
    eos.vm.box = "arista/vEOS-lab-4.17.5M"

    eos.vm.network :forwarded_port, guest: 22, host: 12201, id: 'ssh'
    eos.vm.network :forwarded_port, guest: 443, host: 12443, id: 'https'

    eos.vm.network "private_network", virtualbox__intnet: "s1-l1",
                                      ip: "10.11.0.2", auto_config: false
    eos.vm.network "private_network", virtualbox__intnet: "s2-l1",
                                      ip: "10.21.0.2", auto_config: false
    eos.vm.network "private_network", virtualbox__intnet: "l1-base1",
                                      ip: "10.100.0.1", auto_config: false

    #eos.vm.provision "host_shell", inline: <<-SHELL
    #  ./provision.py eos 12443 vagrant vagrant
    #SHELL

  end

  config.vm.define "leaf2" do |junos|
    junos.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"

    junos.vm.network :forwarded_port, guest: 22, host: 12223, id: 'ssh'

    junos.vm.network "private_network", virtualbox__intnet: "s1-l2",
                                        ip: "10.12.0.2", auto_config: false
    junos.vm.network "private_network", virtualbox__intnet: "s2-l2",
                                        ip: "10.22.0.2", auto_config: false
    junos.vm.network "private_network", virtualbox__intnet: "l3-base300",
                                        ip: "10.33.0.1", auto_config: false
  end

  config.vm.define "leaf3" do |eos|
    eos.vm.box = "arista/vEOS-lab-4.17.5M"

    eos.vm.network :forwarded_port, guest: 22, host: 12203, id: 'ssh'
    eos.vm.network :forwarded_port, guest: 443, host: 12445, id: 'https'

    eos.vm.network "private_network", virtualbox__intnet: "s1-l3",
                                      ip: "10.13.0.2", auto_config: false
    eos.vm.network "private_network", virtualbox__intnet: "s2-l3",
                                      ip: "10.23.0.2", auto_config: false
    eos.vm.network "private_network", virtualbox__intnet: "l3-base2",
                                      ip: "10.200.0.1", auto_config: false

    #eos.vm.provision "host_shell", inline: <<-SHELL
    #  ./provision.py eos 12443 vagrant vagrant
    #SHELL

  end

# http://blog.alainmoretti.com/ios-xrv-working-on-virtualbox/
# https://www.dravetech.com/blog/2016/01/14/vagrant_box_ios_xr.html
# https://github.com/ios-xr/iosxrv-x64-vbox

# https://xrdocs.github.io/application-hosting/blogs/2016-07-12-building-an-ios-xrv-vagrant-virtualbox/
# https://xrdocs.github.io/application-hosting/tutorials/iosxr-vagrant-quickstart

# http://technostuff.blogspot.nl/2008/10/piped-serial-port-on-virtualbox.html

# http://lesser-evil.com/2014/08/ios-xrv-serial-port/

  config.vm.define "spine1" do |junos|
    junos.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"

    junos.vm.network :forwarded_port, guest: 22, host: 12221, id: 'ssh'

    junos.vm.network "private_network", virtualbox__intnet: "s1-l1",
                                        ip: "10.11.0.1", auto_config: false
    junos.vm.network "private_network", virtualbox__intnet: "s1-l2",
                                        ip: "10.12.0.1", auto_config: false
    junos.vm.network "private_network", virtualbox__intnet: "s1-l3",
                                        ip: "10.13.0.1", auto_config: false

#    junos.vm.provision "host_shell", inline: <<-SHELL
#      ./provision.py junos 12220 vagrant vagrant
#    SHELL

  end


  config.vm.define "spine2" do |junos|
    junos.vm.box = "juniper/ffp-12.1X47-D15.4-packetmode"

    junos.vm.network :forwarded_port, guest: 22, host: 12222, id: 'ssh'

    junos.vm.network "private_network", virtualbox__intnet: "s2-l1",
                                        ip: "10.21.0.1", auto_config: false
    junos.vm.network "private_network", virtualbox__intnet: "s2-l2",
                                        ip: "10.22.0.1", auto_config: false
    junos.vm.network "private_network", virtualbox__intnet: "s2-l3",
                                        ip: "10.23.0.1", auto_config: false

#    junos.vm.provision "host_shell", inline: <<-SHELL
#      ./provision.py junos 12220 vagrant vagrant
#    SHELL

  end

  config.vm.provider "virtualbox" do |vb|
  #   # Display the VirtualBox GUI when booting the machine
  #   vb.gui = true
  #
     # Customize the amount of memory on the VM:
     vb.memory = "1548"
  end

#  config.vm.provision "ansible" do |ansible|
#    ansible.verbose = "vv"
#    ansible.playbook = "playbook.yml"
#    ansible.inventory_path = "inventory"
#  end

end
