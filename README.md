记录在Azure上创建VM，安装MQTT+NodeRed_Freeboard服务<br>

# Azure_VM
## **Catalogue**
* [1. Mosquitto](#1)
    * [1.1. Install Mosquitto](#1.1)
    * [1.2. Install Certbot for Let's Encrypt Certificates](#1.2)
    * [1.3. Run Certbot](#1.3)
    * [1.4. Set up Certbot Automatic Renewals](#1.4)
    * [1.5. Configure MQTT Passwords](#1.5)
    * [1.6. Configure MQTT SSL](#1.6)
    
    



##<h2 id="1">1. Mosquitto</h2>
###<h3 id="1.1">1.1 Install Mosquitto</h3>
安装mosquitto: 
```
sudo apt-get install mosquitto mosquitto-clients
sudo apt-add-repository ppa:mosquitto-dev/mosquitto-ppa
sudo apt-get update
```，安装好后可以用mqtt.fx测试一下

###<h3 id="1.2">1.2 Install Certbot for Let's Encrypt Certificates</h3>
Let's Encrypt是一个免费提供SSL证书的服务，利用以下命令安装certbot。
```
sudo add-apt-repository ppa:certbot/certbot
sudo apt-get update
sudo apt-get install certbot
```

###<h3 id="1.3">1.3 Run Certbot</h3>
certbot needs to answer a cryptographic challenge issued by the Let's Encrypt API in order to prove we control our domain. It uses ports 80 (HTTP) and/or 443 (HTTPS) to accomplish this. We'll only use port 80, so let's allow incoming traffic on that port now:<br>
```
sudo ufw allow http
sudo certbot certonly --standalone --standalone-supported-challenges http-01 -d henkeliot.southeastasia.cloudapp.azure.com
```

###<h3 id="1.3">1.4. Set up Certbot Automatic Renewals</h3>
输入```sudo crontab -e```，会先让你选择编辑器，然后再最后一行输入```15 3 * * * certbot renew --noninteractive --post-hook "systemctl restart mosquitto"```。


###<h3 id="1.3">1.5 Configure MQTT Passwords</h3>
```
sudo mosquitto_passwd -c /etc/mosquitto/passwd enter_your_username
sudo nano /etc/mosquitto/conf.d/default.conf
```
接下来在default.conf里复制粘贴以下内容：
```
allow_anonymous false
password_file /etc/mosquitto/passwd
```
再继续输入命令：```sudo systemctl restart mosquitto```，现在可以再到mqtt.fx里测试一下。


###<h3 id="1.3">1.6 Configure MQTT SSL</h3>
继续编辑defalut.conf：```sudo nano /etc/mosquitto/conf.d/default.conf```，进去后再底部添加以下内容：
```
listener 1883 localhost

listener 8883
certfile /etc/letsencrypt/live/mqtt.example.com/cert.pem
cafile /etc/letsencrypt/live/mqtt.example.com/chain.pem
keyfile /etc/letsencrypt/live/mqtt.example.com/privkey.pem
```
然后重启服务```sudo systemctl restart mosquitto```
