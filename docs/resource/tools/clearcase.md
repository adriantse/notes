# ClearCase


## Check License Usage Script

```
@echo off

rem Get the datetime in a format that can go in a filename.
set _my_datetime=%date%_%time%
set _my_datetime=%_my_datetime: =_%
set _my_datetime=%_my_datetime::=%
set _my_datetime=%_my_datetime:/=_%
set _my_datetime=%_my_datetime:.=_%


c:
cd C:\Program Files\IBM\RationalRLKS\common

lmutil.exe lmstat -a -c 19353@asunx009 > d:\tmp\license.%_my_datetime%.txt

lmutil.exe lmstat -a -c 27000@asmet220 >> d:\tmp\license.%_my_datetime%.txt

```



## create view

```
cleartool mkview -tag <branch> /CVS/cc_store/view_store/<branch>.vws
```

## list view

```
cleartool lsvew -s/l
```

## set view

```
cleartool setveiw <view name>
set config spec
cleartool setcs <config spec>
```

## edit config spec

```
cleartool edcs
```

## Find and Remove

```
cleartool find . -depth -name "vssver.scc" -exec  "cleartool
rmelem -f \"%CLEARCASE_PN%\""
```

## Magic file

Edit `C:\Program Files\IBM\RationalSDLC\ClearCase\config\magic`

And add:

```
bmp_image image file : -name "*.[bB][mM][pP]" ;
```

## clearfsimport

### Lets run a preview first
```
clearfsimport -recurse  D:\ClearCase_Storage\exports\has\* C:\Users\sa_clearcase_albdp\sa_clearcase_albdp_view\HealthAssessment
```

### The actual import
```
clearfsimport -recurse -preview  D:\ClearCase_Storage\exports\has\* C:\Users\sa_clearcase_albdp\sa_clearcase_albdp_view\HealthAssessment
```

### Look out for errors 
Most likely its a wrongly name file - leading ClearCase to believe its a text file.

Change it to a compressed_file and check in.

```
$ cleartool chtype -force -nc compressed_file jquery-1.3.2.min.js
```

Run a lsco to verify.

```
$ cleartool lsco -r
```



## SPARC Migration
### Lock ClearCase
```
cleartool lock vob:/usr/dev/ccase/vobs/development/imagine_vob
```

### Sync
```
/usr/local/bin/rsync -a -v --delete /usr/dev/vobstore/imagine_vob.vbs /var/opt/clearcase_dumps
/usr/local/bin/rsync -a -v --delete /usr/dev/vobstore/imagine_vob.vbs /var/opt/sybase_dumps/usg
cd /usr/dev/vobstore/imagine_vob.vbs
rm -rf db.04.30
```

### Reformat
```
/usr/atria/bin/cleartool reformatvob -dump /usr/dev/vobstore/imagine_vob.vbs
```

### Rsync database to emgsyd-clearcase
```
rsync -a -v --delete -a -e "ssh" /usr/dev/vobstore/imagine_vob.vbs emgsyd-clearcase:/data/clearcase/vobstore
#rsync -a -v --delete -a -e "ssh" /usr/dev/vobstore/imagine_vob.vbs emgsydc6n2:/var/opt/clearcase_dumps
```
This will take about 1 hour

### Load
Log into emgsyd-clearcase as clearadm
1. Load the VOB

```
/usr/atria/bin/cleartool reformatvob -load -host emgsyd-clearcase  -hpath /data/clearcase/vobstore/imagine_vob.vbs  /data/clearcase/vobstore/imagine_vob.vbs
```
This will take about 1 hour

2. Cleanup
```
cd /data/clearcase/vobstore/imagine_vob.vbs
rm -rf db.X.X
```

3. Replace the triggers

```
cleartool mktrtype -replace -element -preop MODIFY_DATA,MODIFY_ELEM,MODIFY_MD -all -execunix  /net/emg-nfs-wslauz1prd/vol/vol_vfsydemg01/data_clearcase/triggers/unix/security_preop_all.sh  -execwin "bash \\\\emg-nfs-wslauz1prd\data_clearcase\triggers\win\security_preop_all.sh" EMGDEV_SECURITY
cleartool mktrtype -replace -element -eltype text_file -postop checkout -all -execunix  /net/emg-nfs-wslauz1prd/vol/vol_vfsydemg01/data_clearcase/triggers/unix/sccs_vars_postop_co_trigger.sh -execwin "bash \\\\emg-nfs-wslauz1prd\data_clearcase\triggers\win\sccs_vars_postop_co_trigger_windows.sh" SCCS_KEY_CO

cleartool mktrtype -replace -element -eltype text_file -preop checkin -all -execunix /net/emg-nfswslauz1prd/vol/vol_vfsydemg01/data_clearcase/triggers/unix/sccs_vars_preop_ci_trigger_quant.sh -execwin "bash \\\\emg-nfs-wslauz1prd\data_clearcase\triggers\win\sccs_vars_preop_ci_trigger_quant.sh" SCCS_KEY_QUANT_CI
```

4. Delete the views

```
#!/bin/sh
for x in `/usr/atria/bin/cleartool describe -long
 vob:/data/clearcase/development/imagine_vob | grep uuid | awk '{print $3}' | sed "s/]/ /g"`
do
/usr/atria/bin/cleartool rmview -all -uuid $x
done
```

5. Restart clearcase

```
sudo /opt/VRTSvcs/bin/hares -offline emgsydc6_clearcase_prd_clearcase_web_Application  -sys emgsydc6n1
sudo /opt/VRTSvcs/bin/hares -offline emgsydc6_clearcase_prd_clearcase_Application  -sys emgsydc6n1
sudo /opt/VRTSvcs/bin/hares -online  emgsydc6_clearcase_prd_clearcase_Application  -sys emgsydc6n1
sudo /opt/VRTSvcs/bin/hares -online  emgsydc6_clearcase_prd_clearcase_web_Application -sys emgsydc6n1
```

Log into msgsydapp128 as clearadm

6. Restart clearcase

```
/etc/init.d/clearcase stop
/etc/init.d/clearcase start
```

Test as ladamson on emgsyd-clearcase

1.     Create view
cleartool mkview -tag lee_test -stgloc -auto
...

Test as ladamson on msgsydapp128

Test on PC.

Backout + Leave a read only of old repository.

Log into emsdev1 as clearadm

1.     /usr/atria/bin/cleartool reformatvob -load /usr/dev/vobstore/imagine_vob.vbs
2.     If all has gone well:
cleartool lock vob:/usr/dev/ccase/vobs/development/imagine_vob
else, leave it unlocked.


# CVS to ClearCase Migration
## Setting your environment
```
$ set PATH=%PATH%;C:\Program Files (x86)\cvsnt
$ set CVSROOT=:local:c:\\CVS_Storage\Repository
```

## Transfer CVS repository to migration host
```
$ /export/home/users/msatuser/bin/tar cf Repository.20130612.tar Repository
```


## Running clearexport_cvs
```
cd c:\CVS_Storage\cvs_export\Repository
c:
echo Start: %date%_%time% & clearexport_cvs -V -S -r -o c:\CVS_Storage\cvs-cc-fmbs.dat FMBS   > c:\CVS_Storage\cvs-cc-fmbs.dat.stdout.txt 2>c:\CVS_Storage\cvs-cc-fmbs.dat.stderr.txt & echo Finish: %date%_%time%

Completed.
Converting element ...
Creating element ...
Element "WDRS\WDRS-web\WebDiagram.gph" completed.
Creating script file c:\CVS_Storage\cvs-cc-wdrs.dat ...

```

## Running clearimport
Use any view. clearimport ignores the config spec. Using a dynamic view improves the performance of the import operation.
Set your working directory to the VOB directory in which you plan to import the configuration. You can run clearimport from a location other than the VOB directory by specifying the VOB directory with the –d option. You must specify the pathname of the data file created by clearexport_ssafe

```
clearimport -pcase d:\CVS_Storage\vvs-cc-paycvt.dat
Validating label types.
Validating directories and symbolic links.
Validating elements.
Creating element “.\bugfix/add.sql”.
   version “1”
```
