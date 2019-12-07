# MV1000 install Pi-hole Guide

## Pi-hole

The Pi-hole[®](https://pi-hole.net/trademark-rules-and-brand-guidelines/) is a [DNS sinkhole](https://en.wikipedia.org/wiki/DNS_Sinkhole) that protects your devices from unwanted content, without installing any client-side software.

## MV1000 install Ubuntu System and Environment Preparation

Pls omit step 1 and 2 if you have installed Ubuntu.

1. You can refer the documentation
   <https://docs.gl-inet.com/en/3/app/ssh/>,
   connect to the router with WinSCP, copy the ubuntu system file ubuntu.tar.gz to
   the router tmp directory.

2. You need the ssh router to upgrade to the ubuntu system with the following command:
```
ubuntu_upgrade -n /tmp/ubuntu.tar.gz 
switch_system ubuntu
```
3. Install software soft package
```
sudo apt-get install git
```

4. Disable dnsmasq dns server to solve conflict with pi-hole installation
```
cp /etc/dnsmasq.conf /etc/dnsmasq.conf.bak1
echo "port=0" >>/etc/dnsmasq.conf
systemctl restart dnsmasq
```

## Download Pi-hole Source and Installation Script

1. Download pi-hole source and install
```
git clone --depth 1 https://github.com/pi-hole/pi-hole.git pi-hole
cd pi-hole/automated\ install/
sudo bash basic-install.sh
```
![1](images/1.png)

![2](images/2.png)

![3](images/3.png)

![4](images/4.png)

![5](images/5.png)

![6](images/6.png)

![7](images/7.png)

![8](images/8.png)

![9](images/9.png)

![10](images/10.png)

![11](images/11.png)

![12](images/12.png)

![13](images/13.png)

![14](images/14.png)

![15](images/15.png)

## Enable dhcp server
By default, MV1000 is set as a router for two LAN ports and one WAN port. **dnsmasq** is both a DNS server and a DHCP server, and it was turned off by previous automated installation of pi-hole.

Now we need to start DHCP server for LAN clinets. Here's a dirty hack for enable DHCP server of **dnsmasq** without tackling on the origial **dnsmasq.service**.

- Define a separate config file
```
cat <<EOF >/root/dnsmasq-dhcp-server.conf
interface=br-lan
bind-interfaces
dhcp-range=192.168.8.5,192.168.8.250,255.255.255.0,24h
dhcp-option=option:router,192.168.8.1
port=0
dhcp-option=6,$(cat /etc/dhcpcd.conf  | grep ip_address | cut -f2 -d"=" | cut -f1 -d"/")
EOF
```
You can see above we choose WAN as the working interface for DNS server. So we have "dhcp-option 6" for LAN client to use pi-hole as DNS server.


- Let **dnsmasq** startup at rc.local
```
sed -i 's/^exit 0//' /etc/rc.local

cat <<EOF >>/etc/rc.local
while true; do
	ip link show br-lan && dnsmasq -C /root/dnsmasq-dhcp-server.conf && break
	sleep 2
done &
exit 0
```


## Login Web Page

After the installation completed, enter http://192.168.8.1/admin or <http://192.168.7.141/admin (192.168.8.1 is the router LAN IP, 192.168.7.141 is the router WAN port IP) in the browser to open the login interface.

![16](images/16.png)

SSH to the router to modify the login password with the command

![21](images/21.png)

Then you can log in to the pi-hole management page.

![17](images/17.png)

On the page you can view the network information, you can configure the black and white list and dns, etc.

![18](images/18.png)

![19](images/19.png)

![20](images/20.png)

The background Pi-Hole configuration command can view the official documentation. <https://discourse.pi-hole.net/t/the-pihole-command-with-examples/738>

More configuration information to view <https://docs.pi-hole.net/main/projects/>

