Webmin is a web GUI for administering Linux/Unix systems. The package is available in the official Raspberry OS repos.
```
pi@RPi4:~ $ sudo apt search webmin
Sorting... Done
Full Text Search... Done
webmin/now 1.981 all [installed,local]
  web-based administration interface for Unix systems
```
It can be installed from there by simply running "sudo apt-get install webmin". 
There is another way too to install via the deb package. Following steps need to be taken:

1. Install pre-requisites:
```
pi@RPi4:~ $ sudo apt-get install perl libnet-ssleay-perl openssl libauthen-pam-perl libpam-runtime libio-pty-perl apt-show-versions python
```

2. Get the deb package:
```
pi@RPi4:~ $ wget http://prdownloads.sourceforge.net/webadmin/webmin_1.981_all.deb
```

Be careful though, because by the time you read this, the latest package version could have changed.

3. Install via dpkg:
```
pi@RPi4:~ $ sudo dpkg --install webmin_1.981_all.deb
Selecting previously unselected package webmin.
(Reading database ... 44317 files and directories currently installed.)
Preparing to unpack webmin_1.981_all.deb ...
Unpacking webmin (1.981) ...
Setting up webmin (1.981) ...
Webmin install complete. You can now login to https://RPi4:10000/
```
Access the web interface via https://<YOUR_IP>:10000 in your browser of choice. 

4. The default user is "root". To reset the default password, run the following command:
```
pi@RPi4:~ $ sudo /usr/share/webmin/changepass.pl /etc/webmin root <NEW_PASSWORD>
```
Now you can login with the new password. 
