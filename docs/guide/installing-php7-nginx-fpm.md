Installing composer: https://www.vultr.com/docs/install-composer-on-centos-7
sudo yum install -y zip unzip
Install PHP 7 - https://www.cyberciti.biz/faq/how-to-install-php-7-2-on-centos-7-rhel-7/

```sh
sudo nano /etc/nginx/nginx.conf
sudo vi /etc/opt/remi/php72/php-fpm.d/www.conf
sudo systemctl restart nginx 
sudo systemctl restart php72-php-fpm
sudo usermod pulse -aG nginx
```

```sh
sudo yum install epel-release 
sudo yum update
```

**with root login**

```sh
useradd pulse
passwd pulse
usermod pulse -aG wheel 
```

**Disable root login**

*SSH server settings are stored in the `/etc/ssh/sshd_config` file.*

*To disable root logins, make sure you have the following entry `PermitRootLogin no`*

```sh
sudo sed -i '/#PermitRootLogin yes/c\PermitRootLogin no' /etc/ssh/sshd_config
sudo systemctl restart sshd
```

**Now exit from root prompt and login as pulse user**

**setup IST**

```sh
sudo rm /etc/localtime
sudo ln -s /usr/share/zoneinfo/Asia/Kolkata /etc/localtime
```

**setup hostname ()**
**if hostname doesn't end with domain name**
**mail from system may be rejected**

`sudo hostname $HOSTNAME`

**GIT**

```sh
sudo yum install git
git config --global user.name "$USER $(hostname)"
git config --global user.email "$USER@$(hostname)"
```

**SSH KEYS**

```sh
ssh-keygen -t rsa -b 4096 -C "$(git config --global user.email)"
```

**Ensure ssh-agent is enabled**

```sh
eval "$(ssh-agent -s)"
ssh-add ~/.ssh/id_rsa
```

**remove firewalld**

```sh
sudo systemctl mask firewalld
sudo systemctl stop firewalld
sudo yum remove firewalld
```


## APP SERVER 

**NGINX**

`sudo yum install nginx`

**NODE & NPM**

```sh
sudo yum install nodejs
curl -L https://www.npmjs.com/install.sh | sudo sh
sudo npm i -g n
sudo n lts
```

**PHP**

```sh
sudo rpm -Uvh https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm
sudo rpm -Uvh https://mirror.webtatic.com/yum/el7/webtatic-release.rpm
sudo yum install yum-plugin-replace
sudo yum install php56w php56w-opcache php56w-mysqli php56w-mbstring php56w-fpm php56w-mcrypt php56w-xml
```

**make directory for php session**

```sh
sudo mkdir -p /var/lib/php/session

#give session folder access to pulse**
sudo chown -R pulse:pulse /var/lib/php/session
sudo chown -R pulse:pulse /var/lib/nginx
```

**start at machine start**

```sh
sudo systemctl enable nginx
sudo systemctl enable php-fpm
sudo systemctl start nginx
sudo systemctl start php-fpm
```

```sh
nano /etc/nginx/nginx.conf
user= pulse


nano /etc/php-fpm.d/www.conf
listen.owner = nginx
listen.group = nginx

user = pulse
#; RPM: Keep a group allowed to write in log dir.
group = pulse
```
.com
 User root
 
 
 
 
 
