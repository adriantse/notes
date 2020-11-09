
## System Updates

Keeping the system updated is vital before starting anything on your system. This will prevent people to use known vulnerabilities to enter in your system.

    sudo apt-get update
    sudo apt-get upgrade
    sudo apt-get autoremove
    sudo apt-get autoclean


### Disable Root Account

For security reasons, it is safe to disable the root account. Removing the account might not be a good idea at first, instead we simply need to disable it.

    # To disable the root account, simply use the -l option.
    sudo passwd -l root
    
    # If for some valid reason you need to re-enable the account, simply use the -u option.
    sudo passwd -u root
    

### Secure Shared Memory

Shared memory can be used in an attack against a running service, apache2 or httpd for example. 

    sudo nano /etc/fstab
    : tmpfs	/run/shm	tmpfs	ro,noexec,nosuid	0 0


### IP Spoofing

IP spoofing is the creation of Internet Protocol (IP) packets with a forged source IP address, with the purpose of concealing the identity of the sender or impersonating another computing system.

    sudo nano /etc/host.conf
    : order bind,hosts
    : nospoof on

### SSH

SSH can be very helpful when configuring your server, setup domains or anything else you need to do. It also one of the first point of entry of hackers. This is why it is very important to secure your SSH.

The basic rules of hardening SSH are:
- No password for SSH access (use private key)
- Don't allow root to SSH (the appropriate users should SSH in, then `su` or `sudo`)
- Use `sudo` for users so commands are logged
- Log unauthorised login attempts (and consider software to block/ban users who try to access your server too many times, like fail2ban)
- Lock down SSH to only the ip range your require (if you feel like it)

It is recommended to use SSH keys.

    sudo nano /etc/ssh/sshd_config
    : Port <port>
    : Protocol 2
    : LogLevel VERBOSE
    : PermitRootLogin no
    : StrictModes yes
    : RSAAuthentication yes
    : IgnoreRhosts yes
    : RhostsAuthentication no
    : RhostsRSAAuthentication no
    : PermitEmptyPasswords no
    : PasswordAuthentication no
    : ClientAliveInterval 300
    : ClientAliveCountMax 0
    : AllowTcpForwarding no
    : X11Forwarding no
    : UseDNS no
    
    sudo nano /etc/pam.d/sshd	(comment lines below)
    : #session	optional	pam_motd.so motd=/run/motd.dynamic noupdate
    : #session	optional	pam_motd.so # [1]
    
    sudo service ssh restart