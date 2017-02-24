# Linux Containers

Virtualization technologies like VMWare and VirtualBox take away lot of time and resources. Problem increases when we have to create multiple virtual machines on the same host, delete existing VMs and create new ones.
Linux Containers (LXC), provide a fast and lightweight mechanism to create Linux systems on the same host. 

### Install LXC
`$ sudo apt-get install lxc`

### View list of LXC templates
`$ sudo ls /usr/share/lxc/templates/`

### Create a new Conatiner
`$ sudo lxc-create -n <container-name> -t <template>`

### Start a container
`$ sudo lxc-start -d -n <container-name>`

### Auto Start conatiner when the host machines starts
1. `$ vim /var/lib/lxc/<conatiner-name>/config`
2.  Add entry lxc.start.auto = 1 in this file

### Other usefull LXC commands
1. `$ sudo lxc-ls --fancy`
2. `$ sudo lxc-start --name <conatiner-name> --daemon`
3. `$ sudo lxc-info --name <container-name>`
4. `$ sudo lxc-stop --name <container-name>`
5. `$ sudo lxc-destroy --name <container-name>`
6. `$ lxc-freeze -n <container-name>`
7. `$ lxc-unfreeze -n <container-name>`

### Mount shared directories from host VM to container. 
1. `$ sudo vim /var/lib/lxc/<container-name>/fstab`
2. Add the mount point
3. For example, In case of VMWare host VM - 
  * `/mnt/hgfs      /var/lib/lxc/<container-name>/rootfs/mnt/hgfs        none   bind,create=dir`

### Setup network bridge for LXC containers
1. Make sure yout have bridge-utils
  * `$ sudo apt-get install bridge-utils`
2. Edit your /etc/network/interfaces and disable eth0 by setting it to manual. Add it to br0 by adding a new br0 section and listing eth0 in bridge-ifaces and bridge-ports.
  * auto br0  
iface br0 inet dhcp  
    bridge-ifaces eth0  
    bridge-ports eth0  
    up ifconfig eth0 up  

    iface eth0 inet manual

3. `$ sudo ifup br0`

### Configure LXC to use the bridge
1. `$ sed -i 's/lxc.network.link = lxcbr0/lxc.network.link = br0/' /etc/lxc/default.conf`
2. Now any LXC containers you start with lxc-start will use br0 and get an address from your household DHCP server. They will be accessible from any host in your home.
3. In case of containers created with NAT, 
  * `$ vim /var/lib/lxc/<conatiner-name>/config`
  * Comment or remove `lxc.network.link = lxcbr0`
  * Add `lxc.network.link = br0`
  * Restart the conatiner. Now your container will get the ip address from your host DHCP

### LXC help
1. [Ubuntu help page](https://help.ubuntu.com/lts/serverguide/lxc.html)

2. [Ubuntu server guide](https://help.ubuntu.com/lts/serverguide/lxc.html)

3. [https://www.unixmen.com/setup-linux-containers-using-lxc-on-ubuntu-15-04/](https://www.unixmen.com/setup-linux-containers-using-lxc-on-ubuntu-15-04/)

4. [https://insights.ubuntu.com/2015/11/10/converting-eth0-to-br0-and-getting-all-your-lxc-or-lxd-onto-your-lan/](https://insights.ubuntu.com/2015/11/10/converting-eth0-to-br0-and-getting-all-your-lxc-or-lxd-onto-your-lan/)