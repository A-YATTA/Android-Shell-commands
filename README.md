Some Android shell commands

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

# Automation
## Revoke permissions to App
In this example, it revoke bluetooth permissions to all third party Apps:
```
adb shell 
for package in $(pm list packages -3 | cut -f2 -d":"); do
  pm revoke $package android.permission.BLUETOOTH
  pm revoke $package android.permission.BLUETOOTH_ADMIN 
done
```

## Backup each App individually
Only third party Apps that has backup enabled will be backuped:
```
packages=$(adb -s emulator-5554 shell pm list packages -3 | cut -f2 -d':')
for package in $packages; do
adb backup -apk -f $package.ab $package
adb shell input keyevent 61 # password field
adb shell input keyevent 61 # Do not Backup option
adb shell input keyevent 61 # Backup
adb shell input keyevent 66 # confirm
done
```

## Dump all SMS
``` 
adb shell content query --uri content://sms/
```

## Dump All contacts
```
adb shell content query --uri content://com.android.contacts/data
```


