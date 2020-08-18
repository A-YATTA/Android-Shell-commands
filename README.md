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

## Disable/Uninstall Apps

**Uninstall**
```
pm uninstall PACKAGE_NAME
```

**Disable**
```
pm disable PACKAGE_NAME
```

## Manage System Users

**List users**
```
pm list users
```
**Remove user**
```
pm remove-user USER_ID 
```
USER_ID: from previous command

## Manage accounts (Root access)
```
sqlite3 /data/system_ce/0/accounts_ce.db
sqlite> select * from accounts;
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
> Some usefull settings can be founded in [AMDH](https://github.com/SecTheTech/AMDH/blob/master/config/settings.json) project.

## Managing Apps

**List current running Apps (include Background Apps)**
```
ps -A | grep -E "u0_a[0-9]*" | awk {'print $9'} 
```

**Stop an App**
```
am force-stop PACKAGE_NAME
```

**Start an App**
```
am start -n PACKAGE_NAME
```

**Revoke permission**
```
pm revoke PACKAGE_NAME PERMISSION
```

**Grant permission**
```
pm grant PACKAGE_NAME PERMISSION
```

**Content query**
```
content query --uri content://PACKAGE_NAME/ENTITY
```
Example, get list of contacts:
```
content query --uri content://com.android.contacts/data --projection display_name:data1:data4:contact_id
```

## Dumpsys
**Pending System Update**
```
dumpsys device_policy  | grep "Pending System Update"
```
**Information about package**
```
dumpsys package PACKAGE_NAME
``` 
Information can be:
- Installation date ` grep firstInstallTime `
- Last update date ` grep lastUpdateTime`
- Granted permissions ` grep permission | grep "granted=true"`
- etc...

## Dump current UI
```
uiautomator dump 
```

