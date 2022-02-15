## AWS_NAT_IPTABLES_Ubuntu_Redhat

# AWS_NAT_IPTABLES_Ubuntu 🟦

**For NAT click on your server go to Actions-Networking-Change source/destination check and press the stop square**

![image](https://user-images.githubusercontent.com/98783977/153974740-39e00921-98f1-4361-b6f7-6b586cf171c4.png)

**You need to enable IP forwarding if you want to be capable of forwarding packets to other destinations, go to `nano /etc/sysctl.d/99-sysctl.conf`**

`Uncomment "net.ipv4.ip_forward=1"`

**Do this to check if its correct then save it with**
```
sysctl -w net.ipv4.ip_forward=1
sysctl -p
```
**Install iptables and netfilter-persistent**

`apt install netfilter-persistent iptables-persistent`

**Do POSTROUTING MASQUERADE to allow route traffic without disrupting the original one**
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
systemctl restart iptables
netfilter-persistent save
```
# For the ubuntu client

**Go to `nano /etc/netplan/50-cloud-init.yaml`, if you have DNS installed place the nameservers section if you don't have it don't place it**
```
            dhcp4-overrides:
                use-routes: false
            routes:
              - to: 0.0.0.0/0
                via: 172.31.0.X
                on-link: true
            nameservers:
                search: [inova.pt]
                addresses: [172.31.0.X]
```
**Try and save**
```
netplan try
netplan apply
```
# AWS_NAT_IPTABLES_Redhat 🟥

**For NAT click on your server go to Actions-Networking-Change source/destination check and press the stop square**

![image](https://user-images.githubusercontent.com/98783977/153974740-39e00921-98f1-4361-b6f7-6b586cf171c4.png)

**Install iptables and enable it**
```
yum install iptables-services -y
systemctl enable iptables
systemctl start iptables
```
**Do POSTROUTING MASQUERADE to allow route traffic without disrupting the original one**
```
iptables -t nat -A POSTROUTING -o eth0 -j MASQUERADE
systemctl restart iptables
service iptables save
```
**You need to enable IP forwarding if you want to be capable of forwarding packets to other destinations, go to `/etc/sysctl.conf` and add**

`net.ipv4.ip_forward=1`

**Do this to check if its correct then save it with**
```
sysctl -w net.ipv4.ip_forward=1
sysctl -p
```
