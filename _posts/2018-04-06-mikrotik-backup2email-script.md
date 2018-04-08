---
title: "Essential MikroTik™ scripts #1 :squirrel:"
excerpt: "This script is spawn binary and text configuration file and send it to any email address via GMAIL SMTP server."
categories:
  - how2
tags:
  - mikrotik
  - routeros
  - backup
  - email
  - script
---
Essential MikroTik�~D� scripts #1  :squirrel:  
This script is spawn binary and text configuration file and send it to any email address via GMAIL SMTP server. To achieve it you need 2 steps.
- Register separate gmail account and Allow less secure apps at GMail account settings
- Paste MikroTik script to RouterOS, change user variables and run it  :+1: 

#### Comments
Register your new gmail account and go to My Account > Sign-in & security > Apps with account access and switch ON "Allow less secure apps": [myaccount.google.com/security?utm_source=OGB&utm_medium=act#connectedapps](https://myaccount.google.com/security?utm_source=OGB&utm_medium=act#connectedapps)

This script is undisruptive to your /tool e-mail settings. You can use it on any RouterOS with Internet access, without unexpected changes in your configuration.

```markdown
/log warning "backup_to_EMail is running"

# Don't forget to switch ON "Allow less secure apps" at 
# GMail > My Account > Sign-in & security > Apps with account access
# https://myaccount.google.com/security?utm_source=OGB&utm_medium=act#connectedapps

# GMail full address and password:
:local eAddress "GMailAddress"
:local ePasswd "yourPassword"

# Send backup to this email:
:local eMail "anyEMailAddress"

#Binary and text configuration file names: 
:local BC "bin_conf.backup"
:local TC "text_conf.rsc"

# BEGIN
:local cAddress [/tool e-mail get address]
:local cPort [/tool e-mail get port]
:local cTLS [/tool e-mail get start-tls]
:local cFrom [/tool e-mail get from] 
:local cUser [/tool e-mail get user]
:local cPasswd [/tool e-mail get password]
/tool e-mail set address=smtp.gmail.com port=587 start-tls=yes \
from=$eAddress user=$eAddress password=$ePasswd

/system backup save name=$BC
/export file=$TC
/tool e-mail send file=($TC,$BC) to=$eMail \
subject="$[/system identity get name] Mikrotik backup" \ 
body="$[/system identity get name] backup from $[/system clock get date] \
at $[/system clock get time] o'clock"

/tool e-mail set address=$cAddress port=$cPort start-tls=$cTLS \
from=$cFrom user=$cUser password=$cPasswd
# END

/log warning "backup_to_EMail is done"
```
