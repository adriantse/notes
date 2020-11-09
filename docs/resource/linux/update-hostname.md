Open the /etc/sysconfig/network configuration file in your favorite text editor and change the HOSTNAME entry to reflect the desired system hostname (such as webserver).

```
HOSTNAME=webserver.localdomain
```

```
HOSTNAME=newname.local
cat /etc/sysconfig/network | sed "s/HOSTNAME=.*/HOSTNAME=$HOSTNAME/g" > /tmp/network
sudo cp /tmp/network /etc/sysconfig/network
```

Open the /etc/hosts file in your favorite text editor and change the entry beginning with 127.0.0.1 to match the example below, substituting your own hostname.

```
127.0.0.1 webserver.localdomain webserver localhost localhost.localdomain
```

Reboot the instance to pick up the new hostname.

```
 sudo reboot
```