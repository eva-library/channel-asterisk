
[![N|Solid](https://i.ibb.co/LtT31vK/eva-150px.png)](https://eva.bot/)

# Integration eva Asterisk-Verbio
The following document details the integration of eva with Asterisk and Verbio.
Before starting we must have the following:

## Pre-requisites
- [Google Cloud](https://cloud.google.com/) - Google Cloud Platform account.
- [Asterisk](http://downloads.asterisk.org/pub/telephony/asterisk/) - Download image(iso) of Asterisk (Recommended version 16+).
- [Verbio](https://www.verbio.com/) - Purchase a Verbio license.
- [Buydidnumber](https://www.buydidnumber.com/) - Purchase a telephone number.

## Installation Asterisk

The first thing we must do is go to the Google Cloud console.
```sh
Compute Engine > VM Instaces
```
![N|Solid](https://openil.s3-sa-east-1.amazonaws.com/asteriskverbio/gcloud01.png)

And **Create Instance**.

| Minimum requirements |  |
| ------ | ------ |
| SO | Ubuntu 18+ |
| Machine type | custom (1 vCPU, 7.5 GB memory) |
| Size | 30 GB |
| Disc | SSD persistent disk |
| Mode Boot disc | Boot, read/write |
After created, we will have something similar to this, and by SSH we enter our instance.
![N|Solid](https://openil.s3-sa-east-1.amazonaws.com/asteriskverbio/gcloud02.png)

Install dependencies:
```sh
sudo apt-get install build-essential wget libssl-dev libncurses5-dev libnewt-dev libxml2-dev linux-headers-$(uname -r) libsqlite3-dev uuid-dev git subversion
```
Enter the folder:
```sh
cd /usr/src/
```
Download the latest available version of Asterisk.
For example:
```sh
sudo wget http://downloads.asterisk.org/pub/telephony/asterisk/asterisk-17-current.tar.gz
```
Unzip the downloaded package
```sh
sudo tar -xvzf asterisk-17-current.tar.gz
```
Install additional dependencies available in the installation script that comes with the downloaded Asterisk package
```sh
cd /usr/src/asterisk-16.6.1/contrib/scripts
sudo ./install_prereq install
```
Return to the base folder
```sh
cd /usr/src/asterisk-17.1.1/
```
Execute the following steps to install
```sh
sudo ./configure
sudo make menuselect             
sudo make
sudo make install
sudo make samples               
sudo make config
sudo ldconfig
```
Activate a User in Ubuntu to run the Service.
```sh
sudo groupadd asterisk
sudo useradd -d /var/lib/asterisk -g asterisk asterisk
```
Execute the following instructions to update the "group" and the "user" for the execution of the Asterisk service in the configuration files ...
```sh
sudo sed -i 's/#AST_USER="asterisk"/AST_USER="asterisk"/g' /etc/default/asterisk
sudo sed -i 's/#AST_GROUP="asterisk"/AST_GROUP="asterisk"/g' /etc/default/asterisk
sudo sed -i 's/;runuser = asterisk/runuser = asterisk/g' /etc/asterisk/asterisk.conf
sudo sed -i 's/;rungroup = asterisk/rungroup = asterisk/g' /etc/asterisk/asterisk.conf
sudo chown -R asterisk:asterisk /var/spool/asterisk /var/run/asterisk /etc/asterisk /var/{lib,log,spool}/asterisk /usr/lib/asterisk
```
Restart Ubuntu to verify that the Asterisk Service starts correctly.
```sh
sudo reboot
```
Verify the execution of the Asterisk Service.
```sh
sudo asterisk -vvvvrc
```
Expected output:
![N|Solid](https://openil.s3-sa-east-1.amazonaws.com/asteriskverbio/gcloud03.png)

## Integration Verbio
Integration with verbio is the provider's own and it is they who must provide the respective documentation.

## Integration eva Asterisk+Verbio

> extesions.conf: This file is the one that provides the connection between eva's broker and asterisk. The path is:
```sh
cd /etc/asterisk/extensions.conf
```

It is enough to configure these 2 files to achieve a correct connection between Asterisk and eva.

> eva.agi: Allows you to connect a bot with a phone number. The path is:
```sh
cd /var/lib/asterisk/agi-bin
```

## HELP 
Here are some Asterisk commands that will help you to better understand and configure your VM.
> Enter the Asterisk VM:
```sh
$ sudo asterisk -vvvvvvvvvvvvvvr
```

> In some cases, trying to log in sends the following error message: `Unable to connect to remote asterisk (does /var/run/asterisk/asterisk.ctl exist?)` So the following command will be executed:
```sh
sudo asterisk -&
```
> Restart VM Asterisk:  After modifying any configuration file we must restart the Asterisk VM, for which we execute the following commands:
```sh
$ sudo /etc/init.d/asterisk restart 
$ sudo asterisk -vvvvvvvvvvvvvvr
$ dialplan reload
```
> To review the apps that are installed.
```sh
$ sudo asterisk -vvvvvvvvvvvvvvr 
$ core show applications
```

> The most important files to configure are:

| File | Path | Description |
| ------ | ------ |------ |
| eva.agi | /var/lib/asterisk/agi-bin/eva.agi |where we can connect our bot and associate it with a phone number.|
| verbio.conf | /etc/asterisk/verbio.conf |Contains the configuration properties of verbio.|
| extensions.conf | /etc/asterisk/extensions.conf | All eva connector setup with Asterisk and Verbio|
| dtmb.bnf | /var/lib/asterisk/agi-bin/dtmb.bnf |Contains the verb grammar settings. The path of that file must also be entered in the extensions.conf file|

> Other Important Paths to Check Within Asterisk

- astetcdir => /etc/asterisk
- astmoddir => /usr/lib/asterisk/modules
- astvarlibdir => /var/lib/asterisk
- astdbdir => /var/lib/asterisk
- astkeydir => /var/lib/asterisk
- astdatadir => /var/lib/asterisk
- astagidir => /var/lib/asterisk/agi-bin
- astspooldir => /var/spool/asterisk
- astrundir => /var/run/asterisk
- astlogdir => /var/log/asterisk
- astsbindir => /usr/sbin

Thanks!

**eva professional services**

*Components are not supported
