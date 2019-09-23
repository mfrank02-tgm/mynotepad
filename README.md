# mynotepad
useful stuff for my work and study

### Docker

Docker install Fedora

```
sudo dnf -y install dnf-plugins-core
sudo dnf config-manager \
    --add-repo \
    https://download.docker.com/linux/fedora/docker-ce.repo
sudo dnf config-manager --set-enabled docker-ce-nightly
sudo dnf config-manager --set-enabled docker-ce-test
sudo dnf install docker-ce docker-ce-cli containerd.io
```



Basic and Useful Commands:

```bash
docker search <name>
docker ps -a
docker ps
docker run <name>
docker exec -t -i <name> bash

#Docker Webinterface
docker run -d -p 9000:9000 --name portainer --restart always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer

#postgres database
docker run -d -p 5432:5432 --name postgres -e POSTGRES_PASSWORD=<password> postgres
docker exec -it postgres psql -U postgres

#DNS
docker run --name bind -d --restart=always   --publish 53:53/tcp --publish 53:53/udp --publish 10000:10000/tcp   --volume /srv/docker/bind:/data   sameersbn/bind:latest
```



docker-compose install

```
sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
sudo chmod +x /usr/local/bin/docker-compose
sudo ln -s /usr/local/bin/docker-compose /usr/bin/docker-compose
```



remote mysql

```
mysql --host=<hostname or ip> -u root -p
```



### Raspberry Pi 4
```
#!/bin/bash
ifup eth0
ip addr add 192.168.254.1/24 dev eth0
systemctl restart dhcpd
systemctl restart iptables
nmap -sn 192.168.254.0/27
ssh pi@192.168.254.13
```



DHCP:

```
#
# DHCP Server Configuration file.
#   see /usr/share/doc/dhcp-server/dhcpd.conf.example
#   see dhcpd.conf(5) man page
#
local-address 192.168.254.1;
default-lease-time 600;
max-lease-time 7200;
# If this DHCP server is the official DHCP server for the local
# network, the authoritative directive should be uncommented.
authoritative;
subnet 192.168.254.0 netmask 255.255.255.0 {
        range 192.168.254.10 192.168.254.20;
        option domain-name-servers 10.2.24.153, 9.9.9.9;
        option routers 192.168.254.1;
        #option ip-forwarding off;
        option subnet-mask 255.255.255.0;
        option broadcast-address 192.168.254.255;
}
```



Firewall: 

```
*nat
:PREROUTING ACCEPT [0:0]
:INPUT ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]
:POSTROUTING ACCEPT [0:0]
-A POSTROUTING -o wlan0 -j MASQUERADE
COMMIT
#
*filter
:INPUT ACCEPT [0:0]
:FORWARD ACCEPT [0:0]
:OUTPUT ACCEPT [0:0]

-A INPUT -i lo -j ACCEPT
# fuer bestehende Verbindungen
-A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
# SSH
-A INPUT -m state --state NEW -m tcp -p tcp --dport 22 -j ACCEPT
# HTTP
-A INPUT -m state --state NEW -m tcp -p tcp --dport 80 -j ACCEPT
# HTTPS
-A INPUT -m state --state NEW -m tcp -p tcp --dport 443 -j ACCEPT

-I INPUT -i eth0 -s 192.168.254.0/24 -d 0/0 -j ACCEPT
-I OUTPUT -o eth0 -j ACCEPT

-A FORWARD -i wlan0 -o eth0 -m conntrack --ctstate NEW -m state --state RELATED,ESTABLISHED -j ACCEPT
-A FORWARD -i eth0 -o wlan0 -m conntrack --ctstate NEW -j ACCEPT

# don't react to wrong ports, ignore them
-A INPUT -p tcp -m tcp --syn -j REJECT
-A INPUT -p udp -m udp -j REJECT
#-A INPUT -j REJECT --reject-with icmp-host-prohibited
-A INPUT -j DROP
COMMIT

```

