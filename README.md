记录在Azure上创建VM，安装MQTT+NodeRed_Freeboard服务<br>

# Azure_VM
## **Catalogue**
* [1. Mosquitto](#1)
    * [1.1. Install Mosquitto](#1.1)
    * [1.2. Configure MQTT Passwords & Websockets](#1.2)
* [2. Freeboard](#1)
    * [2.1. Install Apache2 & PHP & Nodejs & Npm](#2.1)
    * [2.2. Install Freeboard](#2.2)
    

<h2 id="1">1. Mosquitto</h2>
<h3 id="1.1">1.1 Install Mosquitto</h3>
Mosquitto完整教程参考：https://www.digitalocean.com/community/tutorials/how-to-install-and-secure-the-mosquitto-mqtt-messaging-broker-on-ubuntu-16-04 但是操作到Step 6最后的时候失败了，1883和8883端口服务都起不来，不知道原因。。所以下面教程忽略SSL/TLS<br>

安装mosquitto: 
```linux
sudo apt-get install mosquitto mosquitto-clients
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
sudo apt-get update
```
安装好后可以用mqtt.fx测试一下



<h3 id="1.2">1.5 Configure MQTT Passwords & Websockets</h3>

```linux
sudo mosquitto_passwd -c /etc/mosquitto/passwd enter_your_username
sudo nano /etc/mosquitto/conf.d/default.conf
```
接下来在default.conf里复制粘贴以下内容：
```linux
allow_anonymous false
password_file /etc/mosquitto/passwd
```
再继续输入命令：
```linux
sudo systemctl restart mosquitto

listener 1883
listener 9001
protocol websockets
```
现在可以再到mqtt.fx里测试一下。


<h2 id="2">2. Freeboard</h2>
<h3 id="2.1">2.1 Install Apache2 & PHP & Nodejs & Npm</h3>

```linux
sudo apt-get update
sudo apt-get install apache2
sudo apt-get install python-software-properties
sudo add-apt-repository ppa:ondrej/php-7.0
sudo apt-get update
sudo apt-get purge php5-fpm
sudo apt-get install php7.0-cli php7.0-common libapache2-mod-php7.0 php7.0 php7.0-mysql php7.0-fpm php7.0-curl php7.0-gd php7.0-bz2
sudo apt-get install nodejs
sudo apt-get install npm
sudo apt-get install nodejs-legacy

```

<h3 id="2.2">2.2 Install freeboard</h3>

```linux
git clone https://github.com/maydaymiao/freeboard.git
cd freeboard
sudo npm -g install grunt
sudo npm install -g grunt-cli
npm install
grunt
sudo -s
cd /var/www/html
ln -s /home/maydaymiao/freeboard dashboard
```


