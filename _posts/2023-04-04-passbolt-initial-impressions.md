---
layout: post
title: "Passbolt Initial Impressions"
date: 2023-04-04
permalink: "/blog/passbolt-initial-impressions"
author: Benjamin Sanders
catagories: passbolt
---
![Image of a text that reads passbolt in black lettering](/assets/images/passbolt.png "passbolt-logo")

[Download as a PDF](/assets/pdfs/Passbolt-Initial-Impressions.pdf)

So, today (4th of April 2023) I decided to spin up a Debian 11 VM to install passbolt to and try out self-hosting a password manager. I currently use 1Password and don’t really see myself switching to a self-hosted solution but figured there is no harm in trying out the “competition”.

The install process so far has been super easy and CLI oriented. I decided to go with the non-Docker install because I like running things directly in the VM. Maybe I’m finally get old -shrug-

Anyway, the process was basically going to https://www.passbolt.com/ce/debian and follow the instructions in CLI.

```bash
wget "https://download.passbolt.com/ce/installer/passbolt-repo-setup.ce.sh" 
wget "https://github.com/passbolt/passbolt-dep-scripts/releases/latest/download/passbolt-ce-SHA512SUM.txt" 
sha512sum -c passbolt-ce-SHA512SUM.txt && sudo bash ./passbolt-repo-setup.ce.sh || echo "Bad checksum. Aborting" && rm -f passbolt-repo-setup.ce.sh
sudo apt install passbolt-ce-server
```

One thing I notice is that it installs certbot implying I should be using a domain and not attempting to this locally. Let’s see what happens when I try it locally anyway.

Okay, it’s going to start off creating an empty database.

![](/assets/images/configuring-passbolt-mysql.png)

Saved the MySQL user and password in 1Password 😆 then got to the TLS section and hit none since I don’t have a domain to use currently. Looks like I can provide the IP of the server instead though which I’m entering in 192.168.1.173.

![](/assets/images/configuring-passbolt-domain-name.png)

Setting up PHP 7.4 and MariaDB 10.5 so a little out-of-date on the PHP but not too bad. We’re done!

![](/assets/images/configuring-passbolt-done.png)

Let’s go the server’s IP and see what it looks like now and get it configured.

Cool as mentioned in the wiki I get the not yet configured page [https://help.passbolt.com/hosting/install/ce/debian/debian.html](https://help.passbolt.com/hosting/install/ce/debian/debian.html)

![](/assets/images/configuring-passbolt-not-configured-yet.png)

Entering in the database information

![](/assets/images/configuring-passbolt-db-connection.png)

🤔 Hmm getting that it can’t connect to the database.

![](/assets/images/passbolt-configuration-no-connection.png)

I know it’s online and I can connect.

```bash
ben@passbolt-test:~$ mysql -u passboltadmin -p
Enter password:
Welcome to the MariaDB monitor.  Commands end with ; or \g.
Your MariaDB connection id is 41
Server version: 10.5.18-MariaDB-0+deb11u1 Debian 11

MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| passboltdb         |
+--------------------+
```

I know for sure MariaDB is running.

```bash
ben@passbolt-test:~$ sudo systemctl status mariadb.service
● mariadb.service - MariaDB 10.5.18 database server
	 Loaded: loaded (/lib/systemd/system/mariadb.service; enabled; vendor preset: enabled)
	 Active: active (running) since Tue 2023-04-04 11:47:04 EDT; 11min ago
```

Found it!

I was trying to use the server’s public IP and needed the local IP `127.0.0.1`.

Source: [https://community.passbolt.com/t/trying-to-configure-passbolt-database-configuration/5322](https://community.passbolt.com/t/trying-to-configure-passbolt-database-configuration/5322)

Got it going and got a neat message setting up. Lmao

![](/assets/images/configuring-passbolt-elon.png)

Tried setting up the iOS app but guess it won’t work without a TLS certificate of some kinds which makes sense. [https://community.passbolt.com/t/oops-something-went-wrong-when-trying-to-set-up-mobile-transfers/5124](https://community.passbolt.com/t/oops-something-went-wrong-when-trying-to-set-up-mobile-transfers/5124)

![](/assets/images/configuring-passbolt-mobile-setup.png)

All-in-all I’m going to keep trying out Passbolt but don’t see myself switching from 1Password to self-hosted but might be good to have a local backup.