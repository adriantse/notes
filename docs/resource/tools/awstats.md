## install required package

ipkg install perl

## apache configuration

```
<IfModule log_config_module>
   LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\"" combined
   LogFormat "%h %l %u %t \"%r\" %>s %b" common
               LogFormat "%{Referer}i -> %U" referer
               LogFormat "%{User-agent}i" agent
   <IfModule logio_module>
     LogFormat "%h %l %u %t \"%r\" %>s %b \"%{Referer}i\" \"%{User-Agent}i\" %I %O" combinedio
   </IfModule>
   #
   # If you prefer a logfile with access, agent, and referer information
   # (Combined Logfile Format) you can use the following directive.
   #
</IfModule>
CustomLog logs/access.log combined
```

### awstats configuration

```
LogFile="/usr/local/apache/logs/access.log"
```
