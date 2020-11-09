```
cp  /etc/sysctl.conf /tmp/sysctl.conf

echo "
# Custom Tibco config
#TCP/IP	Maximum OS receive buffer size for all connection types.
net.core.rmem_max=8388608

#TCP/IP	Default OS receive buffer size for all connection types.
net.core.rmem_default=65536

#TCP/IP	Maximum OS send buffer size for all connection types.
net.core.wmem_max=8388608

#TCP/IP	Default OS send buffer size for all types of connections.
net.core.wmem_default=65536

#TCP/IP	Enable/Disable TCP/IP window scaling enabled?
net.ipv4.tcp_window_scaling=1

#TCP/IP	TCP auto-tuning setting:
net.ipv4.tcp_mem=8388608 8388608 8388608

#TCP/IP	TCP auto-tuning (receive) setting:
net.ipv4.tcp_rmem=4096 87380 8388608

#TCP/IP	TCP auto-tuning (send) setting:
net.ipv4.tcp_wmem=4096 65536 8388608

#TCP/IP	This will ensure that immediately subsequent connections use these values.
net.ipv4.route.flush=1

# TCP	How may times to retry before killing alive TCP connection. RFC1122 says that the limit should be longer 
net.ipv4.tcp_retries2=10


# Linux	Committing virtual memory II:
vm.overcommit_ratio=25

# Generally speaking an enterprise server should not need to swap out pages in order to make room for the file buffer cache or other processes which would favor a setting of 0.  
vm.swappiness=25
" >> /tmp/sysctl.conf

sudo cp /etc/sysctl.conf /etc/sysctl.conf.`date +"%Y%m%d"`
sudo cp /tmp/sysctl.conf /etc/sysctl.conf

sudo sysctl -p /etc/sysctl.conf
 
```

```
sudo rm /etc/security/limits.d/90-nproc.conf

echo "
tibco          soft    nofile    65000
tibco          soft    nproc    65000
tibco          hard    nproc    65000
tibco          hard    nproc    65000

*          soft    nofile    4096
*          soft    nproc     4096

root       soft    nofile    unlimited
root       soft    nproc     unlimited" > /tmp/90-nproc.conf

sudo cp /tmp/90-nproc.conf /etc/security/limits.d/90-nproc.conf
```