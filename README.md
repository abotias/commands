# Commands
List of useful commands I've needed at a time.


## BASH
### Add time to history
```sh
for i in $(ls /home);do echo '"export HISTTIMEFORMAT=%d/%m/%y %T "' >> /home/$i/.bash_profile ;done && echo 'export HISTTIMEFORMAT="%d/%m/%y %T "' >> ~/.bash_profile
```
### Show services actives
```sh
systemctl list-units --type=service --status=active
```


### Generate certificates
CSR:
```sh
openssl genrsa -out abotias.key 4096 
openssl req -out abotias.csr -key  abotias.key -new -sha256
```
Selfsigned:
```sh
sudo openssl req -x509 -nodes -days 1000 -newkey rsa:2048 -keyout /etc/ssl/private/selfsigned.key -out /etc/ssl/certs/selfsigned.crt
sudo openssl dhparam -out /etc/ssl/certs/dhparam.pem 2048
```


### IPTABLES

show traffic
```sh
watch -n1 iptables -nvL
```

Edit rules sequence
```sh
iptables-save > /tmp/iptables.txt
nano /tmp/iptables.txt
iptables-restore < /tmp/iptables.txt
```

Drop IP
```sh
iptables -A OUTPUT -d X.X.X.X/32 -j DROP
```

## ESXi
### Copy VM to other ESXI

```sh
scp -rv $virtual_machine root@10.10.10.5:/vmfs/volumes/4a4b4e49-1b8fad213-0000-000000000000
```

### ovftool
- Extract VM
~~~
.\ovftool.exe -ds=<datastore> vi://root@<ip>/<virtual_machine> C:\Users\<user>\<folder>\
~~~
- Deploy VM
~~~
C:\Program Files\VMware\VMware OVF Tool\ovftool.exe' --name="$name" -dm=thin --datastore="<datastore>" C:\Users\<user>\Desktop\$template\$template.ovf vi://@<ip>/
~~~

### Disable raw disk scanning at ESXi boot
```sh
esxcli storage core device setconfig -d naa.1010101010101010100000000000000e --perennially-reserved=true
```

## Raspberry Pi
### Get power supply status
```sh
vcgencmd get_throttled
```

The bits on the returned number mean:

Bit Hex value Meaning.\
0 1 Under-voltage detected.\
1 2 Arm frequency capped.\
2 4 Currently throttled.\
3 8 Soft temperature limit active.\
16 10000 Under-voltage has occurred.\
17 20000 Arm frequency capping has occurred.\
18 40000 Throttling has occurred.\
19 80000 Soft temperature limit has occurred.
