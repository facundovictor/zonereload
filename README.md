# zonereload
Reloads a bind dns zone without the need to restart the named/bind9 service.

**Description:**

Assuming the zone to update is called “THEZONE” perform the following:
```
    vim /var/data/named/data/zones/THEZONE.zone;
```
Update the zone in vim and save it. Then you would need to:
```
    rndc freeze THEZONE
    rndc reload THEZONE
    rndc thaw THEZONE
```
Other sysAdmins/devOps/partners would just use:
```
    service named restart
```
Doing so will clear its cache (if used as a local DNS resolver) and cause a
small outage if clients do not have their current domain lookup cached.

So… the moral of the story is to freeze, reload and thaw. But for uniformize
and standardize the reload method through the entire team i developed this
script that also sends and email alerts.

**Installation:**
Upload the script to the server to */bin/zonereload*
```
    chown root root /bin/zonereload
    chmod 744 /bin/zonereload
```
**Mail configuration:**
The email alerts needs a valid email-address, to configure it edit /bin/zonereload
and modify the mail_account parameter at the very top of the file:
```
   mail_account="postmaster@mydomain.com.ar"
```
Save it and you are ready to use it!

**Usage:**
```
    zonereload -z <zone>
```
Options:

- "-h" to show the help.
- "-s" to avoid email notification.
- "-z <zone>" to specify the zone.
