# CVS

### Tools

- cvsspam - http://www.badgers-in-foil.co.uk/projects/cvsspam/
- viewvc - http://www.viewvc.org/
- cvschangelogbuilder - http://cvschangelogb.sourceforge.net/


### Solaris Service

```
svcs cvspserver/tcp
```

```
STATE          STIME    FMRI
online         Nov_27   svc:/network/cvspserver/tcp:default
```

### Configuration
```
cat /etc/services | grep pser
cvspserver      2401/tcp # CVS Client/server operations
cvspserver stream tcp nowait root /usr/local/bin/cvs cvs -f --allow-root=/apps/cvsrep/Repository pserver
```

## cvs diff

### Full diff

```
cvs diff -N -c -r RELEASE_1_0 -r RELEASE_1_1 
```

### File Listing
```
cvs diff -N -c -r RELEASE_1_0 -r RELEASE_1_1 | grep "Index:
 " | sed 's/Index: //g'
```

### Generate Change log

Obtain cvschangelogbuilder from http://cvschangelogb.sourceforge.net/

1. check out or go to the working directory

2. run the following command to generate change logs

```
cvschangelogbuilder.pl -output=listdeltabyfile -module=myproject -tagstart=myproj_2_0 -d=john@cvsserver:/cvsdir
   
```

## To generate changelog between two tags:
```
cvschangelogbuilder.pl -output=buildhtmlreport:nosummary,nolimit,notags,nolinesofcode,nodaysofweek,nohours,nodevelopers,includediff -module=Module1 -tagstart=TAG1  -tagend=TAG2 --viewcvsurl=http://viewvchost:49152/viewvc/cvsroot/Module1  > ~/Module1 .html
```
