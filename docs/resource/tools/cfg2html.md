cfg2html

# Download

```
wget ftp://rpmfind.net/linux/dag/redhat/el6/en/x86_64/dag/RPMS/cfg2html-1.79-1.el6.rf.noarch.rpm

sudo rpm -vi cfg2html-1.79-1.el6.rf.noarch.rpm

rm ./cfg2html-1.79-1.el6.rf.noarch.rpm
```

# Setup default information

```
rm /tmp/systeminfo
cat <<EOF >> /tmp/systeminfo
Company:        Sydney Trains
Location:       AWS
URL:            https://console.aws.amazon.com/ec2

EOF

sudo cp /tmp/systeminfo /etc/cfg2html/systeminfo
```

# Setup cron

```
touch crontab.txt
echo "30 4 * * 7 sudo /usr/bin/cfg2html -o /tmp"  >>  crontab.txt
crontab crontab.txt
rm crontab.txt
```

# setup your first run

```
sudo /usr/bin/cfg2html -o /tmp
```
