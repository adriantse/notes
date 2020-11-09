```
# nsupdate
> server ns.mydns.com
> update delete oldhost.example.com. A
> update add newhost.example.com. 86400 A 192.168.254.117
> send
> quit
```

```
for host in `echo $dnshosts`
do
        ip_add=`cat $CONF_FILE | grep $host| awk '{print $1}'`
        record_type=`cat $CONF_FILE | grep $host| awk '{print $3}'`

        # Do some regex to see if it's an IP or Hostname
        if [ $(echo $host | egrep -o '^[0-9]+.[0-9]+.[0-9]+.[0-9]+') ]
        then
        # Its an IP, lookup the PTR record
        records=$(nslookup $host | grep 'name = ' | awk -F' = ' '{print $2}' | sed 's/.$//g' | sort)
        else
        # Its a hostname, lookup the A record
        records=$(nslookup $host | grep -A1 'Name:' | grep Address | awk -F': ' '{print $2}')
        fi

        # Were there any records returned?
        if [ "$records" == "" ]
        then
                echo "$host not found.  Needs update."
                echo "update add $host 86400 $record_type $ip_add
send" | nsupdate -d
        else
                echo "$records found"
                # Does dns match our records or config file?
                if [ "$record_type" == "A" ]
                then
                        if [ "$records" == "$ip_add" ]
                        then
                                echo "$ipadd matched dns"
                        else
                                echo "config file: $ipadd"
                                echo "dns: $records"
                                echo "update add $host 86400 $record_type $ip_add
send" | nsupdate -d
                        fi
                fi  # not a CNAME
        fi
done
```