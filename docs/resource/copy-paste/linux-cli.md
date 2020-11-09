
## Create random file
```
dd if=/dev/urandom bs=1M count=1000 of=/tmp/big_file
```

```
for i in `seq 1 10`
do
	sudo dd if=/dev/urandom bs=1M count=20 of=/tmp/file.$i &
done
```

## Select the nth argument of a command
Consider the following scenario:

```
$ ls /var/www/website/images/large/
```


This will list out the files but now you want to switch to that directory. 
You wouldn't want to type that path again, this is how you do this in bash:

```
$ cd !!:1
```

## arrays
```
names=( Adrian Simon Sam )
```

```
for name in ${names[@]} do
  echo $name
  # other stuff on $name
done
```