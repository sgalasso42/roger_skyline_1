# roger_skyline_1

## VM Network and Security setup

### Package installation
`&> su` - to be sudo<br>
`&> apt-get update && apt-get upgrade` - get the latest update and install it<br>
`&> apt-get install sudo iptables-persistent apache2 portsentry` - install the necessary
### Setup sudo rights
`&> sudo usermod -aG sudo username` - make username sudo<br>
`&> sudo - username` - switch to this user<br>
`&> sudo whoami` - if `root` is send to the terminal, then it worked !
### Use a static IP with a /30 netmask
open the /etc/network/interfaces file
replace `dhcp` by `static`<br>
add `address your_new_ip`<br>
add `netmask 255.255.255.252`<br>
add `gateway your_gateway`
`&> sudo reboot` - restart the VM<br>
then try to `ping` to check if youd did it right
### Setup SSH access
open the /etc/ssh/sshd_config file<br>
uncomment `#Port 22` and change `22` to a value > 1024 and < 65535<br>
`&> sudo service ssh restart` - restart the services
Try to connect by SSH to check if it work
`PermitRootLogin no` - prohibit SSH root access<br>
`PubkeyAuthentication yes` - grant SSH key access<br>
`&> sudo service ssh restart` - restart the services
### Publickeys SSH access
On the client machine<br>
`&> ssh-keygen` - to generate a ssh key<br>
`&> ssh-copy-id username@your_ip -p your_ssh_port` - to generate a ssh key<br>
if `Number of key(s) added: 1` has been send to the terminal, then it's good<br>
`&> ssh username@your_ip -p your_ssh_port` - try to connect with ssh key<br>
open the /etc/ssh/sshd_config file<br>
`PasswordAuthentication no` - prohibit SSH password access<br>
`&> sudo service ssh restart` - restart the services
retry to connect with SSH to check if it work
### Iptables rules and port security
