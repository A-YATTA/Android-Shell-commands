# Android-stuff
Some Android commands and codes

# ADB Shell Commands
## list installed packages:
```
pm list packages
```
By default the command list enabled applications (-e).
**List Third party Apps**
```
pm list packages -3 
```
**List System Apps**
```
pm list packages -s 
```
**Disabled Apps**
```
pm list packages -d 
```

Sometimes the command `pm list packages` does not return all installed applications. `dumpsys package` list also all installed applications:
```
dumpsys package | grep "Package \[" | cut -d "\[" -f2 | cut -d "\]" -f1
```
## Manage Settings

SECTION can be: secure, global or system
**list**
```
settings list SECTION
```
**Add/Edit Key**
```
settings put SECTION KEY VALUE
```

