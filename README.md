### Work in progress!
A simple Clos network Vagrant lab provisioned via Ansible and NAPALM

- `ansible-galaxy install -r requirements.yml` to install requirements (currently only NAPALM)
- `vagrant up spine1 spine2 leaf1 leaf2 leaf3` to start up all networking devices
- `vagrant up base1 base2` to start both servers
- `ansible-playbook -i inventory playbook.yml` provision lab

In current setup:
spine1 = JunOS
spine2 = JunOS
leaf1 = Arista vEOS
leaf2 = JunOS
leaf3 = Arista vEOS
base1 = Ubuntu 16.04 LTS
base2 = Ubuntu 16.04 LTS

Vargant boxes:
JunOS = juniper/ffp-12.1X47-D15.4-packetmode
Arista vEOS = arista/vEOS-lab-4.17.5M
Ubuntu = ubuntu/xenial64

Vagrant version 1.9.4
Ansible version 2.4.0
