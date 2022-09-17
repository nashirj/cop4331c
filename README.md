# setting up LAMP stack
https://www.digitalocean.com/community/tutorials/how-to-install-linux-apache-mysql-php-lamp-stack-on-ubuntu-20-04

# setting up mysql
https://www.digitalocean.com/community/tutorials/how-to-install-mysql-on-ubuntu-20-04#step-3-creating-a-dedicated-mysql-user-and-granting-privileges

# viewing web app from other device (e.g. mobile phone)
`hostname -I` to get ip address, then go to `http://<myip>:80`

# having multiple lamp apps
to switch lamp server domains pointed to by localhost, edit `/etc/apache2/sites-available/000-default.conf` to point the `DocumentRoot` to the correct directory.
Then, run this command:
```
sudo a2dissite 000-default && sudo a2ensite 000-default && systemctl reload apache2
```

# local development
You might need to update [firefox settings to clear cache automatically during development](https://support.mozilla.org/en-US/questions/905902)

when you get json decode error, problem is probably due to database connection; to fix have to update .htaccess or hardcode values into Database.php

db credentials for local development
$this->hostname = "localhost";
$this->username = "nashir";
$this->password = "110395";
$this->dbName = "COP4331";

Sometimes, firefox aggressively caches; need to do Ctrl + shift + r to get rid of old js files