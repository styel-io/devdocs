# MERN Stack

## What is MERN stack?

| MERN means |                                                  |                                     |
| ---------- | ------------------------------------------------ | ----------------------------------- |
| M          | [MongoDB](https://www.mongodb.com/)              | NoSQL Database                      |
| E          | [Express](https://www.npmjs.com/package/express) | Web framework for NodeJS            |
| R          | [React](https://reactjs.org/)                    | JavaScript Library                  |
| N          | [Nginx](https://nginx.org/)                      | Web Server and Reverse Proxy Server |

## AWS

### 1. EC2 인스턴스 임대

<figure>
  <img width="888" alt="AWS EC2  인스턴스 임대" src="https://user-images.githubusercontent.com/14961047/62196292-b693ba80-b3b8-11e9-9ac1-b31e16bc3dde.png">
  <figcaption>Ubuntu Server 18.04 LTS 이미지를 선택</figcaption>
</figure>

<figure>
 <img width="902" alt="인스턴스 유형 선택" src="https://user-images.githubusercontent.com/14961047/62196406-ee9afd80-b3b8-11e9-8c2c-2e48d50d5bdd.png">
  <figcaption>사양을 선택하고 검토 및 시작 버튼을 누른다</figcaption>
</figure>

<figure>
 <img width="1087" alt="인스턴스 시작 검토" src="https://user-images.githubusercontent.com/14961047/62196727-7e40ac00-b3b9-11e9-8cdd-2f997454ae98.png">
  <figcaption>선택한 인스턴스 사양을 검토하고 보안 그룹 편집을 선택하자</figcaption>
</figure>

<figure>
 <img width="1087" alt="인스턴스 시작 검토" src="https://user-images.githubusercontent.com/14961047/62196727-7e40ac00-b3b9-11e9-8cdd-2f997454ae98.png">
  <figcaption>선택한 인스턴스 사양을 검토하고 보안 그룹 편집을 선택하자</figcaption>
</figure>

<figure>
 <img width="1091" alt="보안 규칙 설정" src="https://user-images.githubusercontent.com/14961047/62196953-f27b4f80-b3b9-11e9-8068-c07234f2d98f.png">
  <figcaption>80, 443 포트를 열어준다</figcaption>
</figure>

<figure>
<img width="680" alt="키 페어 생성" src="https://user-images.githubusercontent.com/14961047/62197208-69184d00-b3ba-11e9-8782-0f385c17eb40.png">
  <figcaption>보안 접속에 필요한 키를 생성하여 로컬에 저장한다</figcaption>
</figure>

### 2. EC2 인스턴스 연결

<figure>
<img width="1091" alt="인스턴스 연결" src="https://user-images.githubusercontent.com/14961047/62197495-e3e16800-b3ba-11e9-8964-8d2ea4f62bd9.png">
  <figcaption>인스턴스를 선택하고 연결을 누른다</figcaption>
</figure>

<figure>
<img width="806" alt="인스턴스 연결" src="https://user-images.githubusercontent.com/14961047/62197672-3f135a80-b3bb-11e9-8ab5-632726f57e0e.png">
  <figcaption>위 내용대로 실행한다</figcaption>
</figure>

### 3. EC2 인스턴스 접속

<figure>
<img width="806" alt="인스턴스 접속" src="https://user-images.githubusercontent.com/14961047/62201443-63bf0080-b3c2-11e9-99db-47adc4e97f3b.png">
  <figcaption>정상적으로 접속이 되었다</figcaption>
</figure>

## How to install MERN stack on Ubuntu 18.04

### Introduction

### Prerequisites

Ubuntu 18.04 initial server setup.

#### 1. Set up user password

```bash
sudo passwd
```

#### 2. Install OpenSSH

```bash
sudo apt-get install openssh-server
```

#### 3. Set up a basic firewall

```bash
sudo ufw app list

# allow OpenSSH protocol
sudo ufw allow OpenSSH

# enable firewall
sudo ufw enable
```

[ufw guide](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands)

## Step 1: Install Nginx Reverse Proxy Server

### 1. Installing nginx on Linux

```bash
sudo apt update

sudo apt install nginx
```

### 2. Adjusting the Firewall

```bash
sudo ufw allow 'Nginx Full'
```

### 3. Checking Web Server

```bash
sudo systemctl status nginx

curl -4 [IP Adress or DNS]
```

`http://your_server_ip`  
You should see the default Nginx landing page

### 4. Managing the Nginx Process

```bash
# To stop your web server
sudo systemctl stop nginx

# To start the web server when it is stopped
sudo systemctl start nginx

# To stop and then start the service again
sudo systemctl restart nginx

# If you are simply making configuration changes, Nginx can often reload without dropping connections. To do this
sudo systemctl reload nginx

# By default, Nginx is configured to start automatically when the server boots. If this is not what you want, you can disable this behavior
sudo systemctl disable nginx

# To re-enable the service to start up at boot
sudo systemctl enable nginx
```

### 5. Create a server block

```bash
sudo vim /etc/nginx/sites-available/styel.io
```

```bash
# /etc/nginx/sites-available/styel.io

server {
        listen 80;
        listen [::]:80;
root /var/www/html;
        index index.html index.htm;
server_name styel.io www.styel.io;

location / {
        proxy_pass http://localhost:3000;
        proxy_http_version 1.1;
        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection 'upgrade';
        proxy_set_header Host $host;
        proxy_cache_bypass $http_upgrade;
    }
}
```

```bash
sudo ln -s /etc/nginx/sites-available/styel.io /etc/nginx/sites-enabled/

sudo unlink /etc/nginx/sites-enabled/default

sudo ln -s /etc/nginx/sites-available/default /etc/nginx/sites-enabled/
```

```bash
sudo vim /etc/nginx/nginx.conf
```

```bash
# /etc/nginx/nginx.conf

# Uncomment the code below
server_names_hash_bucket_size 64;
```

```bash
sudo nginx -t

sudo systemctl restart nginx
```

### 6. SSL

<https://www.digitalocean.com/community/tutorials/how-to-secure-nginx-with-let-s-encrypt-on-ubuntu-18-04>

```bash
# add the repository
sudo add-apt-repository ppa:certbot/certbot

# Install Certbot's Nginx package with apt
sudo apt install python-certbot-nginx
```

```bash
sudo vim /etc/nginx/sites-available/styel.io
```

```bash
# /etc/nginx/sites-available/styel.io
server_name styel.io www.styel.io;
```

```
sudo certbot --nginx -d styel.io -d www.styel.io
```

```bash
sudo certbot renew --dry-run
```

## Step 2: Install Express for NodeJS

### 1. Set up nvm package

<https://github.com/nvm-sh/nvm>

- Install & Update script

```bash
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash

#or

wget -qO- https://raw.githubusercontent.com/nvm-sh/nvm/v0.34.0/install.sh | bash
```

```bash
export NVM_DIR="$HOME/.nvm"
[ -s "$NVM_DIR/nvm.sh" ] && \. "$NVM_DIR/nvm.sh"  # This loads nvm
[ -s "$NVM_DIR/bash_completion" ] && \. "$NVM_DIR/bash_completion"  # This loads nvm bash_completion
```

- Verify installation

```bash
command -v nvm
```

- List available versions

```bash
nvm ls-remote
```

### 2. Install NodeJS

```bash
# latest release
nvm install node

# specific version
nvm install 6.14.4 # or 10.10.0, 8.9.1, etc
```

### 3. Start the project

- Make project directory

```bash
mkdir styel_node

cd styel_node
```

- Install Yarn package manager

```bash
npm install -g yarn
```

```
yarn init
```

### 4. Install ExpressJS module

```bash
# global module
yarn global add express

yarn add express
```

```bash
yarn install
```

### 5. App.js

```js
const express = require("express");
const app = express();
const port = 3000;

app.listen(port, () => {
  console.log(`Express Server has started on port ${port}`);
});
```

### 6. Start Express Server

```bash
node app.js
```

## Step 3: Install MongoDB

<http://mongodb.github.io/node-mongodb-native/3.1/installation-guide/installation-guide/>

### 1. MongoDB Pulbic GPG Key 등록

```bash
sudo apt-key adv --keyserver hkp://keyserver.ubuntu.com:80 --recv 9DA31620334BD75D9DCB49F368818C72E52529D4
```

### 2.MongoDB 를 위한 list file 생성

```bash
 echo "deb [ arch=amd64 ] https://repo.mongodb.org/apt/ubuntu bionic/mongodb-org/4.0 multiverse" | sudo tee /etc/apt/sources.list.d/mongodb-org-4.0.list
```

### 3. Install

```bash
sudo apt-get update

# latest stable version 설치
sudo apt-get install -y mongodb-org
```

### 4. Start MongoDB Server

```bash
sudo service mongod start

# 서버가 제대로 실행됐는지 확인
sudo cat /var/log/mongodb/mongod.log
```

### 5. Adjusting the Firewall

```bash
sudo ufw allow 27017/tcp
```

### 6. Connect MongoDB Server

```bash
mongo
```

## Step 4: Install React

### 1. Install React

```bash
npm install -g create-react-app

create-react-app styel_react
```

```bash
cd styel_react
```

### 2. Start

```bash
yarn start
```

---

## PM2

<http://pm2.keymetrics.io/>

### 1. 설치

```bash
sudo npm install pm2@latest -g
```

### 2. 실행

```bash
pm2 start yarn -- run dev
```

## Docker

<https://docs.docker.com/install/linux/docker-ce/ubuntu/>

```bash
# uninstall old versions
sudo apt-get remove docker docker-engine docker.io containerd runc

# SET UP THE REPOSITORY
sudo apt-get update

sudo apt-get install \
    apt-transport-https \
    ca-certificates \
    curl \
    gnupg-agent \
    software-properties-common

# Add Docker’s official GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

# Verify that you now have the key with the fingerprint 9DC8 5822 9FC7 DD38 854A E2D8 8D81 803C 0EBF CD88, by searching for the last 8 characters of the fingerprint.
sudo apt-key fingerprint 0EBFCD88

sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
```

### INSTALL DOCKER CE

```bash
sudo apt-get update

sudo apt-get install docker-ce docker-ce-cli containerd.io

# List the versions available in your repo:
apt-cache madison docker-ce

# sudo apt-get install docker-ce=<VERSION_STRING> docker-ce-cli=<VERSION_STRING> containerd.io
sudo apt-get install docker-ce=5:18.09.7~3-0~ubuntu-bionic docker-ce-cli=5:18.09.7~3-0~ubuntu-bionic containerd.io
```

## VSFTPD

```bash
# 설치
sudo apt-get install vsftpd

# vsftpd.conf 설정

sudo vim /etc/vsftpd.conf

anonymous_enable=NO
local_enable=YES
write_enable=YES

# (ftp서버 시작)
sudo service vsftpd start

# (ftp서버 상태확인)
sudo service vsftpd status
```

VScode에서 원격 파일 제어
VScode extensions에서 sftp by liximomo 검색

`Cmd+Shift+P` 커맨드 팔레트를 열고 `SFTP: config`를 선택

sftp.json

VScode extensions에서 sftp by liximomo 검색

`Cmd+Shift+P` 커맨드 팔레트를 열고 `SFTP: config`를 선택

sftp.json

```json
{
  "name": "styel",
  "host": " ip or DNS ",
  "protocol": "sftp",
  "port": 22,
  "username": "ubuntu",
  "password": " your password ",
  "privateKeyPath": " your key path ",
  "remotePath": "/home/ubuntu/",
  "uploadOnSave": true,
  "ignore": [".vscode", ".git", ".DS_Store", "node_modules"]
}
```

---

## Reference

<http://mongodb.github.io/node-mongodb-native/3.1/quick-start/quick-start/>
