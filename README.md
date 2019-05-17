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
add `gateway your_gateway`<br>
`&> sudo reboot` - restart the VM<br>
then try to `ping` to check if youd did it right
### Setup SSH access
open the /etc/ssh/sshd_config file<br>
uncomment `#Port 22` and change `22` to a value > 1024 and < 65535<br>
`&> sudo service ssh restart` - restart the services<br>
Try to connect by SSH to check if it work<br>
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
`&> sudo service ssh restart` - restart the services<br>
retry to connect with SSH to check if it work
### Iptables rules and port security
make a script in `/etc/network/if-pre-up.d/` that may :<br>
exemple in the `iptables_script` file in the repo<br>
- flush the table<br>
- drop everything<br>
- make rules against DOS attack<br>
- keep established connections input / output<br>
- accept localhost input / output<br>
- accept ssh input / output<br>
- accept http input / output<br>
- accept https input / output<br>
`&> sudo chmod /etc/network/if-pre-up.d/your_script_name` - grant boot exec<br>
`&> sudo reboot` - restart vm and try connections to check you did it well
### Anti port scan security
https://fr-wiki.ikoula.com/fr Se_prot%C3%A9ger_contre_le_scan_de_ports_avec_portsentry
### Try DOS attack and port scan on your VM
Make a new virtual machine (you can clone this one, but don't forgot to change the ip)<br>
`&> sudo git clone https://github.com/gkbrk/slowloris.git` - get slowloris<br>
`&> sudo python3 slowloris.py target_ip:target_port`<br>
if your target VM is still working then it work
### Try a port scan on your VM
Make a new virtual machine (you can clone this one, but don't forgot to change the ip)<br>
`&> sudo apt-get install nmap` - get nmap<br>
`&> sudo nmap target_ip`<br>
if nmap is not finding port, then it's good
### Remove useless services
`&> sudo apt-get remove name_of_service` - remove the service<br>
`&> sudo apt-get purge name_of_service` - purge the service
### Scripts
make a script to update packages<br>
edit /etc/crontab to run it on boot, reboot and each week at 4am<br>
make a script to check Crontab file modifications<br>
edit /etc/crontab to run it each day at midnight<br>
Exemple in the `crontab_script` and `update script` in the repo
