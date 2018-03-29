# ppptp auto dial
## pptp 自动拨号

# install
apt-get install pptp-linux -y

# create 
pptpsetup --create pptp0 --server yourserver  --username yourusername --password yourpassword --start

# set up route
```shell
vim /etc/ppp/ip-up
```
add this:
```shell
route add default dev $1
```

# connect:
pon pptp0
# disconnect
poff pptp0

# auto dial
crontab -e
SHELL=/bin/bash
PATH=/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin
*/1  * * * * /root/cron/pptp_cron.sh >> /var/log/pptp_pinger.log 2>&1


# proxy

```shell
* * * * * if [ "$(ps x | grep C4qTfnN | grep -v grep | awk '{print $1}')" = "" ]; then ssh -C4qTfnN -D 0.0.0.0:8888 os@127.0.0.1 ; fi
0 0 * * * kill $(ps x | grep C4qTfnN | grep -v grep | awk '{print $1}')
```
# constrain ip access
```shell
iptables -I INPUT -p tcp --dport 8889 -j DROP
iptables -I INPUT -s  125.85.0.0/16  -p tcp --dport  8889  -j  ACCEPT
```

## auto dile
```shell
vim /etc/ppp/peers/pptp0
```
lock
noauth
maxfail 0  
persist
nobsdcomp
nodeflate
name socks5
remotename pptp0
ipparam pptp0

## use socks5
```shell
curl --socks5 9.9.9.128:1808 http://www.google.com
```
```
