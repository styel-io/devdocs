# MERN Stack

## What is MERN stack?

| MERN means |                                                  |                                     |
| ---------- | ------------------------------------------------ | ----------------------------------- |
| M          | [MongoDB](https://www.mongodb.com/)              | NoSQL Database                      |
| E          | [Express](https://www.npmjs.com/package/express) | Web framework for NodeJS            |
| R          | [React](https://reactjs.org/)                    | JavaScript Library                  |
| N          | [Nginx](https://nginx.org/)                      | Web Server and Reverse Proxy Server |

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

## Reference

<http://mongodb.github.io/node-mongodb-native/3.1/quick-start/quick-start/>
