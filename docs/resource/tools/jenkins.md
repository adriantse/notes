## Update default ports on OSX

## add the default parameters

```
sudo defaults write /Library/Preferences/org.jenkins-ci httpPort 7070
```

## stop

```
sudo launchctl unload /Library/LaunchDaemons/org.jenkins-ci.plist
```

## start

```
sudo launchctl load /Library/LaunchDaemons/org.jenkins-ci.plist
```
