记录在Azure上创建VM，安装MQTT+NodeRed_Freeboard服务<br>

# Azure_VM
## **Catalogue**
* [1. Mosquitto](#1)
    * [1.1. Install Mosquitto](#1.1)
    * [1.2. Configure MQTT Passwords](#1.2)

    
    

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



<h3 id="1.2">1.5 Configure MQTT Passwords</h3>
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
```
现在可以再到mqtt.fx里测试一下。


