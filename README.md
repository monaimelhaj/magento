# Installation magento 2

### I. Intro

#### 1.1- Requeierement:

1. OS: Linux Ubuntu v18
2. web server: Nginx v1.17
3. Mysql: mysql server v14
4. Magento v2.3.2


 

### II. Preparation

1. connect as root
```
sudo -i
```

2. mettre a jour system
```
apt update && apt upgrade
```

3. Creation user et le dossier html
3.1 Creation user `magento`
```
adduser magento
```
3.2 Creation dossier `html`
```
mkdir /home/magento/html
```
3.3. modifier les droits
```
chown -R magento:magento /home/magento/html
```

4. Creation Access Keys<br>
4.1 Connecter (Inscrit) sur <a href="https://marketplace.magento.com" target="_blank"><b>Magento Marketplace</b></a>.<br>
4.2 Allez sur <b>votre compte</b> (Haut droit) et choisi <b>My Profile</b>.<br>
4.3 Cliqur sur <b>Access Keys</b> (tab Marketplace).<br>
![Image description](docs/static/1.png)<br>
4.4 Cliqur sur <b>Create A New Access Key</b> (Verrifier que vous etes toujours sur tab magento 2).<br>
4.5 Ecrivez un nom de votre cle puis OK.<br>
4.6 Gardez les <b>Public Key</b> et <b>Private Key</b>

5. installation zip
```
apt-get install zip unzip
```

### III- Installation FTP Server

##### Installation
installer vsftp
```
apt install vsftpd -y
```


##### Configuration
prendre une copie de fichier de config
```
cp /etc/vsftpd.conf /etc/vsftpd.conf.orig
```
ouvrer le fichier de config sur nano
```
nano /etc/vsftpd.conf
```

Uncomment `local_enable` et checker si la valeur `YES` (To allow the local users to log in to the FTP server).
```
local_enable=YES
```

Uncomment `write_enable` et checker si la valeur `YES` (to allow the FTP write command).
```
write_enable=YES
```

chercher le parametre `chroot_local_user` et modifier les parametres pour que sa soit:
```
user_sub_token=$USER
chroot_local_user=YES
```
uncomment `chroot_list_enable` et checker que la valeur `YES`
```
chroot_list_enable=YES
```
uncomment la line 
```
chroot_list_file=/etc/vsftpd.chroot_list
```

et ajouter les lignes:
```
local_root=/home/$USER/html
allow_writeable_chroot=YES
```

uncomment la line 
```
ls_recurse_enable=YES
```

sauvegarder et quiter<br>

redemerrer service
```
service vsftpd restart
```
##### Ajouter l'utilisateur `magento`
```
echo "magento" | sudo tee -a /etc/vsftpd.userlist
```
```
cat /etc/vsftpd.userlist
```
> *output*<br>magento

##### Tester
```
ftp -p localhost
```
> root@ubuntu-s-4vcpu-8gb-fra1-01:/home/cosmos# ftp -p localhost<br>
Connected to localhost.<br>
220 (vsFTPd 3.0.3)<br>
Name (localhost:root): cosmos<br>
331 Please specify the password.<br>
Password:<br>
230 Login successful.<br>
Remote system type is UNIX.<br>
Using binary mode to transfer files.<br>
ftp>


	

### IV- Installation PHP & extensions




Installez le package <b>software-properties-common</b> pour nous fournir le package <b>add-apt-repository</b>
```
apt-get install software-properties-common
```

mettre à jour votre système avec des packages non pris en charge à partir de <b>ppa</b>
```
add-apt-repository ppa:ondrej/php
```

Mettre à jour les packages
```
apt-get update
```


`
[Optionnel] La commande apt-cache search peut effectuer des recherches de packages (si vous avez d'autres packages a installer).
`
```
apt-cache search php7.2
```

install les packages
```
apt-get install php7.2 libapache2-mod-php7.2 php7.2-common php7.2-gd php7.2-mysql php7.2-curl php7.2-intl php7.2-xsl php7.2-mbstring php7.2-zip php7.2-bcmath php7.2-soap php-xdebug php-imagick
```

afficher la versin du PHP installer
```
php -v
```

a la fin de l'installation, on trouves que apache est installe<br>pour le supprimer

```
apt-get remove apache2
```

netoyage total
```
apt-get purge apache2 apache2-utils apache2.2-bin apache2-common
```

### V- Installation et configuration du PHP-FPM (<a href="https://www.php.net/manual/en/install.fpm.php ">FastCGI Process Manager</a>)

##### Installation

Install PHP 7.2 FPM:
```
apt-get install php7.2-fpm
```

##### Configuration

prendre une copie du fichier de config
```
cp /etc/php/7.2/fpm/php.ini /etc/php/7.2/fpm/php.ini.backup
```

Ouvrir le fichier de config sur nano<br>
```
nano /etc/php/7.2/fpm/php.ini
```

avec la commande <b>Ctrl + W</b> on fait des recherches sur le fichier, et modifier les valeurs:
- memory_limit = 2G<br>
- max_execution_time = 3600<br>
- max_input_time = 1800<br>
- upload_max_filesize = 10M<br>
- zlib.output_compression = On<br>

`Ctrl + W: recherche`<br>
`Ctrl + O: sauvegarde`<br>
`Ctrl + X: quit`<br>

Redemarrer le service
```	
service php7.2-fpm start
```	
	

### VI- Installation Composer

### VII- Installation web server: nginx

### VIII- Installation MySql Server

### IX- Installation PhpMyAdmin

### X- Installation Java

### XI- Installation elasticsearch

### XII- Installation Firewall

### XIII- Installation Magento
