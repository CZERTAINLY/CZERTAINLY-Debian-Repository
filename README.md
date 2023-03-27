# CZERTAINLY Debian Repository

CZERTAINLY Appliance Debian Repository - script for managing repository with czertainly-appliance-tools package.

## Initial setup

1. clone repository
2. edit `base` path in [`manage-repository`](manage-repository) script to match your need
3. prepare PGP key and put private into `/root/CZERTAINLY.deb.pgp-key.private` or configure alternative path in [`manage-repository`](manage-repository)
4. create virtual host in apache
```
<VirtualHost *:80>
    ServerName deb.tomasek.cz

    Redirect   permanent / https://deb.tomasek.cz/

    ErrorLog        /var/log/apache2/deb.tomasek.cz-error.log
    LogLevel        warn
    CustomLog       /var/log/apache2/deb.tomasek.cz-access.log combined
</VirtualHost>

MDomain deb.tomasek.cz

<VirtualHost *:443>
    ServerName deb.tomasek.cz

    DocumentRoot /var/www/deb.tomasek.cz/debian-repository

    SSLEngine on
    Header always set Strict-Transport-Security "max-age=63072000"

    <Directory "/">
        AllowOverride None
        Require all granted
    </Directory>

    Options +FollowSymLinks

    ErrorLog /var/log/apache2/deb.tomasek.cz-error.log
    LogLevel warn
    CustomLog /var/log/apache2/deb.tomasek.cz-access.log combined
</VirtualHost>
```

## add package to repository 
```
./manage-repository add bullseye /tmp/czertainly-appliance-tools_2.6.0~rc6_all.deb
```

## rebuild 
Rebuild isn't normaly needed. All files are being regenerated when package is added. But in case you rename repository or change key you might find this useful.
```
./manage-repository rebuild
```
