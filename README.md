# Commands
List of useful commands I've needed at a time.


## BASH
### Add time to history
```sh
for i in $(ls /home);do echo "export HISTTIMEFORMAT=»%d/%m/%y %T "' >> /home/$i/.bash_profile ;done && echo 'export HISTTIMEFORMAT="%d/%m/%y %T "' >> ~/.bash_profile
```
### Show services actives
```sh
systemctl list-units --type=service --status=active
```

### Get all software and version installed
Debian/Ubuntu:
```sh
apt-mark showmanual|while read a;do dpkg -l $a|tail -n1|awk '{print $2";"$3}';done
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
## Python
### Share folder with http server
```sh
python3 -m http.server 8001
```

## TEMP
```sh
netstat -tuplan|awk 'NR>2'|grep -v ESTABLISHED|grep -v "127.0.0.53"|sed 's/0.0.0.0://g'|sed 's/LISTEN//g'|awk '{print $1";"$4";"$6$7}'
ps aux|grep "$(netstat -tuplan|awk 'NR>2'|grep -v ESTABLISHED|grep -v "127.0.0.53"|sed 's/0.0.0.0://g'|sed 's/LISTEN//g'|awk '{print $6}'|cut -d"/" -f1)"|grep -v grep
apt-mark showmanual|while read a;do dpkg -l $a|tail -n1|awk '{print $2";"$3}';done
cat /etc/passwd|grep -v "nologin"|grep -v "false"|grep -v "sync"
for i in $(systemctl list-units --type=service --state=active --no-pager|grep -v "systemd-fsck"|grep -v "systemd-"|awk 'NR>1'|head -n -5|awk '{print $1}');do for j in $(systemctl status $i --no-pager|grep "Loaded"|cut -d"(" -f2|cut -d";" -f1);do cat $j| if grep -q "ExecStart";then cat $j|grep "ExecStart"|sed 's/ExecStart=//g'|awk -v h=$i '{print h";"$1}';else echo "===========================================================$i";fi;done;done
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
