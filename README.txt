# Install Mageia

1.-![mageia1](https://raw.githubusercontent.com/carlosbrevisperez/imagenesmageia/master/mageia/Screenshot_1.png)

2.-![mageia2](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_2.png?raw=true)

3.-![mageia3](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_3.png?raw=true)

4.-![mageia4](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_4.png?raw=true)

5.-![mageia5](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_5.png?raw=true)

6.-![mageia6](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_6.png?raw=true)

7.-![mageia7](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_7.png?raw=true)

8.-![mageia8](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_8.png?raw=true)

9.-![mageia9](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_9.png?raw=true)

10.-![mageia10](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_10.png?raw=true)

11.-![mageia11](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_11.png?raw=true)

12.-![mageia12](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_12.png?raw=true)

13.-![mageia13](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_13.png?raw=true)

14.-![mageia14](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_14.png?raw=true)

15.-![mageia15](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_15.png?raw=true)

16.-![mageia16](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_16.png?raw=true)

17.-![mageia17](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_17.png?raw=true)

18.-![mageia18](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/mageia/Screenshot_18.png?raw=true)



# Create user sudo

Three steps

* Install sudo
* Verify sudo installation
* Give privileges


##### Install sudo

```javascript 
# /usr/sbin/urpmi sudo
```
##### Verify sudo installatio

```javascript 
$ rmp -q sudo > /dell/null && echo sudo is intalled || echo sudo not installed
sudo is intalled
```
**Now you have to give your user or group sudo priviledges**

To do this, you must edit the / etc / sudoers file, as root. With the command to edit visudo, which opens the file / etc / sudoers in your default text editor.

```javascript
#visudo
```
To give root access to user "carlos", add this line in /etc/sudoers file

```javascript
carlos ALL=(ALL) ALL
```
# Network settings

This command allows us to see the network cards that we have

```javascript
If config -a 
```
##### Configure network cards

Enter the file and add these lines

```javascript
#nano/etc/network/interfaces
```

Within the text editor you must write

```javascript

auto eth1


iface eth1 inet dhcp

```

Finally we must restart the network to apply the changes :+1:



# Install and enable web server

### To install all typical packages, use the command **task-lamp**

This will install the following key applications:

* Apache: the web server
* PHP: language to generate dynamic pages
* MariaDB: a database server
* PERL: a programming language

#### Create local domain for development and test in your home directory

By default, the files are located in "/var/www/html/" which requires root access.

Create a directory for that, for instance /home/your_user_name/www
Within, create one folder per website. For instance, mysite_test and mysite_dev
Set up the permissions to all with command the

```javascript
chmod -R 777 www
```
To start the server once installed, go to the MCC / System / Manage system services by enabling or disabling them and start httpd by default, **the server will always start when starting your computer**.

By default, the files are located in /var/www/html/ which requires root access.

create a directory for that, for instance /home/user_name/www

```javascript
$mkdir -p /home/jaime/www
```
Within, create one folder per website. For instance, "mysite_test" and "mysite_dev"

```javascript
$mkdir  /home/jaime/www/mysite_test

$mkdir  /home/jaime/www/mysite_dev
```

#### Now edit apache web server configuration

```javascript
cd /etc/httpd/conf

cp httpd.conf httpd.conf.bak
```
##### Change the default document root of your web server

```javascript
vi httpd.conf
```
##### Change the lines

```javascript 
DocumentRoot /home/jaime/www

 #
 # Relax access to content within /var/www.
 #
 <Directory /home/jaime/www>
     AllowOverride None
     # Allow open access:
     Require all granted
 </Directory>
 
 # Further relax access to the default document root:
 <Directory /home/jaime/www>
```

##### Start a console and change user to be root

```javascript
<Directory /home/jaime/www/mysite_dev>
     Allow from all
 </Directory>
 <Directory /home/jaime/www/mysite_test>
     Allow from all
 </Directory>
```
##### Create the redirection from the host name

```javascript
#
 <VirtualHost 127.0.0.1>
      DocumentRoot /home/jaime/www/mysite_dev
      ServerName mysite_dev
 </VirtualHost>
 #
 <VirtualHost 127.0.0.1>
      DocumentRoot /home/jaime/www/mysite_test
      ServerName mysite_test
 </VirtualHost>
```
##### Restart the server: go to the MCC / System / Manage system services by enabling or disabling them and stop and start the service httpd.

![falló](https://raw.githubusercontent.com/carlosbrevisperez/imagenesmageia/master/ssh/1.png)
 
To test this is working, type mysite_dev in your browser :+1:


# Installation, enabling and configuration of the firewall 

The firewall GUI in Mageia Control Centre (drakfirewall) is a front end for the Shoreline Firewall more commonly known as Shorewall

By manually editing shorewall files, it is possible to create much more specific firewalls in addition to the simple firewall provided by drakfirewall.

This trick is to create 2 list, the _"Blacklisting"_ and _"Whitelisted"_ in which some programs will be accessed or blocked

### Blacklisting

We have to make use of a feature in shorewall called ipsets which is a dynamic list of IP address ranges. Ipsets depends on a package called ** xtables **, so we should start by installing some packages. Open a terminal and enter your to become the root user

```javascript
# urpmi xtables-addons xtables-geoip xtables-addons-kernel-desktop-latest
```
##### We can confirm that Ipsets are available in shorewall with the command

```javascript
# shorewall show capabilities | grep Ipset
   Ipset Match (IPSET_MATCH): Available
```
##### Now we create a script which will download a list of country IP address ranges and put them into an ipset. 

##### Use a text editor (nano) to create a file called /usr/local/bin/ipset-geoblock-country.sh

```javascript
#!/bin/bash

ipset -exist create geoblock hash:net
ipset flush geoblock

wget -O /tmp/GeoIPCountryCSV.zip -T 5 -t 1 http://geolite.maxmind.com/download/geoip/database/GeoIPCountryCSV.zip 
unzip  -p /tmp/GeoIPCountryCSV.zip | tr -d '"' |  cut -d"," -f5,1-2  | awk -F, '\
BEGIN 	{

#Define list of countries to block
Countries = "CN BR";

split(Countries, countries, " ");
 } \
{
for (i in countries) {
                if (countries[i] == $3) {
                        system("ipset -A geoblock " $1 "-" $2 " -exist");
                }
        }
}	\
' 
ipset save geoblock > /etc/shorewall/geoblock
rm -f /tmp/GeoIPCountryCSV.zip
```

##### Make the script executable

```javascript
#chmod +x /usr/local/bin/ipset-geoblock-country.sh
```
**important to set up a cron job to run once a month**

```javascript
#cd /etc/cron.monthly
ln -s /usr/local/bin/ipset-geoblock-country.sh
```

##### Run the script manually and view the contents of the ipset we have created

```javascript
/usr/local/bin/ipset-geoblock-country.sh
ipset list | more
```

##### Now to configure shorewall to blacklist these ip address ranges edit the file /etc/shorewall/blrules

```javascript
#ACTION		SOURCE			DEST			PROTO	DEST	SOURCE		ORIGINAL	RATE		USER/	MARK	CONNLIMIT	TIME         HEADERS         SWITCH
#									PORT	PORT(S)		DEST		LIMIT		GROUP
DROP	net:+geoblock		all
```
_"DROP	net:+geoblock		all"_
this line means that all packages coming from the Internet with the source IP address contained in the ipset called **"geoblock"** will be deleted.

##### Finally we must configure shorewall to load the ipset from the saved file when it starts.

```javascript
# restore geographical blacklist if present
if [ -f /etc/shorewall/geoblock ]; then
  ipset destroy geoblock
  ipset -file /etc/shorewall/geoblock restore geoblock
fi
```

##### To activate the new configuration use the command

```javascript
shorewall safe-restart
```

**With this will compile the firewall rules we create and will highlight any errors in the configuration, and will ask we  confirmation before activating them.**

### Whitelisted

##### Countries can be "whitelisted" in a manner similar to the previous blacklist. First we create a script to create an ipset.

```javascript
/usr/local/bin/ipset-whitelist-country.sh
```
```javascript
#!/bin/bash

ipset -exist create geowhitelist hash:net
ipset flush geowhitelist

wget -O /tmp/GeoIPCountryCSV.zip -T 5 -t 1 http://geolite.maxmind.com/download/geoip/database/GeoIPCountryCSV.zip 
unzip  -p /tmp/GeoIPCountryCSV.zip | tr -d '"' |  cut -d"," -f5,1-2  | awk -F, '\
BEGIN 	{

#Define list of countries to whitelist
Countries = "GB";

split(Countries, countries, " ");
 } \
{
for (i in countries) {
                if (countries[i] == $3) {
                        system("ipset -A geowhitelist " $1 "-" $2 " -exist");
                }
        }
}	\
' 
ipset save geowhitelist > /etc/shorewall/geowhitelist
rm -f /tmp/GeoIPCountryCSV.zip
```

**edit /etc/shorewall/rules**

```javascript
#ACTION		SOURCE		DEST		PROTO	DEST	SOURCE		ORIGINAL	RATE		USER/	MARK	CONNLIMIT	TIME         HEADERS
#							PORT	PORT(S)		DEST		LIMIT		GROUP
ACCEPT	net:+geowhitelist	fw	tcp	80,443
ACCEPT		net:192.168.1.0/24	fw	tcp	22,80,443
Ping(ACCEPT)	net:192.168.1.0/24	fw
#SECTION ALL
#SECTION ESTABLISHED
#SECTION RELATED
#INCLUDE	rules.drakx
```
**The line _"ACCEPT	net:+geowhitelist	fw	tcp	80,443"_ opens ports 80 and 443 (http and https) only to users in the geowhitelist ipset, the last line to include the drakfirewall configuration is commented out to stop drakfirewall from opening ports without using the whitelist.**

##### Now configure shorewall to load the ipset when it restarts.

```javascript
# Restore whitelist from file
if [ -f /etc/shorewall/geowhitelist ]; then
  ipset destroy geowhitelist
  ipset -file /etc/shorewall/geowhitelist restore geowhitelist
fi
```
##### To activate the new configuration use the command

```javascript
#shorewall safe-restart
```

# Configuration of the system clock

To configure the respective time zone of the place where we are, we will have to carry out the following steps.

**we use this syntax**

hwclock set date="year-month-day hours:minutes:seconds"

```javascript
# hwclock --set --date="2017-11-25 16:25"
```
Now, to synchronize the system time with the BIOS time, we execute the this command

```javascript
# hwclock --hctosys
```

Para poder comprobar el funcionamiento, ejecutamos el comando _"date"_ o _"hwclock"_

```javascript
# date

or

# hwclock
```

**important if the computer is restarted, the date and time of the BIOS will be taken**


# How does the grub boot system modifications work?


When installing "mageia" the default "grub2-efi" was installed

Every time you start the computer you will run "grub.efi", it acts according to your configuration file called grub.cfg located in / boot / grub2

There is a tool called grub-mkconfig that will allow us to edit the existing script or create new ones and then update the menu

##### This is the list of commands

* 00_header, es la secuencia de comandos que carga la configuración de GRUB desde / etc / default / grub , incluido el tiempo de espera, la entrada de inicio predeterminada y otros. 
* 10_linux, carga las entradas del menú para la distribución instalada. 
* 20_linux_xen, 
* 20_ppc_terminfo, 
* 30_os-prober, es la secuencia de comandos que escaneará los discos duros para otros sistemas operativos y los agregará al menú de inicio. 
* 40_custom, es una plantilla que puede usar para crear entradas adicionales que se agregarán al menú de inicio. 
* 41_personalizado 
* 90_persistente 
* 93_memtest, carga la utilidad memtest.


The numbering in the names of the script defines precedence. This means that _"10_linux"_ will be executed before _"30_os-prober_" and, therefore, will be placed higher in the order of the start menu. 

Like the "/boot/grub2/grub.cfg" file, they are not intended to be edited, except _"40_custom_" and _"41_custom"_. You must be very careful when working with these scripts. The scripts must be executed. This means that they have the execution bit activated. If you want to disable a script, do not delete it, disable the execution bit and it will not be executed.

You can place as many files as you want in the /etc/grub.d directory, all executable shell scripts will be read in their numbering order.

**NOTE:** The main configuration file is / etc / default / grub, it is analyzed by the 00_header script.

# Install SSH service

**1.Select the options**
+ Choose Expert for all options

![SSH1](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/1.png?raw=true)


**2.General options**

+ Set the options for visibility and root access. Port 22 is the standard SSH port.

![SSH2](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/2.png?raw=true)

**3.Authentication Methods**

+ Allow a variety of authentication methods that users can use during the connection, click Next.

![SSH3](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/3.png?raw=true)


**4.Login**

+ Choose registration facility and output level, then click Next.


![SSH4](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/4.png?raw=true)


**5.Options Login**

+ Set options for each login, click Next.

![SSH5](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/5.png?raw=true)

**6.User Login Options**

+ Configure the user access options, then click Next


![SSH6](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/6.png?raw=true)

**7.Compression and Forwarding**
+ Configure X11 forwarding and compression during transfer, then click Next.

![SSH7](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/9.png?raw=true)

**8.Summary**

+ Take a second to check these options, then click Next.

![SSH8](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/7.png?raw=true)

**9.Finalize**

+ It is done! Click Finish


![SSH9](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ssh/8.png?raw=true)

# Install FTP service

**1.Introduction**

+ The first page is just an introduction, click Next.

![FTP1](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ftp/1.png?raw=true)


**2.Selecting exposure server: Local and / or World Network**

+ Exposing the FTP server to the Internet has its risks. Be prepared for bad things.


![FTP2](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ftp/2.png?raw=true)

**3.Server Information**
	
+ Enter the name with which the server will be identified, email address for complaints and if login access to root should be allowed.


![FTP3](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ftp/3.png?raw=true)


**4.Server Options**

+ Set listening port, caged user, permission sheets and / or FXP (File eXchange Protocol)

![FTP4](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ftp/4.png?raw=true)


**5.Summary**

+ Take a second to check these options, then click Next.

![FTP5](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ftp/5.png?raw=true)


**6.Finalize**

+ It is done! Click on Finish

![FTP6](https://github.com/carlosbrevisperez/imagenesmageia/blob/master/ftp/6.png?raw=true)
