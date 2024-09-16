# GUIDE (tested on debina 12.7)

1. Log in root
```bash
su -
```

2. Add username into /etc/sudoers
```bash 
nano /etc/sudoers
```
### Our text (username - you have to change the username on your own)
username ALL=(ALL) NOPASSWD:ALL

3.
## Install dependecies 
```bash
apt install git curl wget gnupg2
```
  ### Add ansible repository and installing
```bash
wget -qO- "https://keyserver.ubuntu.com/pks/lookup?fingerprint=on&op=get&search=0x6125E2A8C77F2818FB7BD15B93C4A3FD7BB9C367" | gpg --dearmor | sudo tee /usr/share/keyrings/ansible-archive-keyring.gpg > /dev/null
```
```bash
echo "deb [signed-by=/usr/share/keyrings/ansible-archive-keyring.gpg] http://ppa.launchpad.net/ansible/ansible/ubuntu jammy main" | sudo tee /etc/apt/sources.list.d/ansible.list
```
```bash
apt update
```
```bash
apt install ansible
```
```bash
exit
```

4. Get from github playbook for preparing your computer.
```bash
git clone https://github.com/k109ru/demo.git
``` 

5. Configure ansible playbook for you computer.
   Configure variables (path is defaults/main.yml)
Setup your ip address, username, what kind of software you want to install and other.
```bash
cd demo
```

Get your network interface
```bash
ip a | grep enp
```
output example
``` bash 
2: enp1s0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel master br0 state UP group default qlen 1000
```
Your network interface is enp1s0 (in this case)

```bash
nano my_pc_prepare/defaults/main.yml
```

Example:
```yml
---
# Set up software for our pc
install_browsers: true
install_social_networks: true
install_media_tools: true
install_development_tools: true
install_system_utils: true

# your username in the system 
user: "user"

#for kvm
name_of_interface: enp1s0
libvirt_group: "libvirt"
user_to_add: "user"
name_of_the_bridge: "br0"
ip4_and_mask: "192.168.1.10/24"
gw4: "192.168.1.1"
dns4: "192.168.1.1 8.8.8.8"
```

6. Run our ansible playbook  
```bash
sudo ansible-playbook -K install_localhost.yml
``` 