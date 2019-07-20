# Ubuntu 18.04 LTS initail server setup

## Step 1: Set up user password

```bash
sudo passwd
```

## Step 2: Set up a basic firewall

### 1. Install ufw package

```bash
sudo apt-get update

sudo apt-get install ufw
```

### 2. Make firewall rules

```bash
sudo ufw app list

sudo ufw allow OpenSSH
```

### 3. Enable firewall

```bash
sudo ufw enable
```

You can learn some common UFW operations in [this guide](https://www.digitalocean.com/community/tutorials/ufw-essentials-common-firewall-rules-and-commands).
