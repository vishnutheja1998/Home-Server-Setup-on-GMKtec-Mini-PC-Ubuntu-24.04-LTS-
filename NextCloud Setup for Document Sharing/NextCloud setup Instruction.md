## ☁️ Nextcloud Setup on Ubuntu with Docker, Nginx, and HTTPS

This guide walks through setting up a self-hosted **Nextcloud** server on Ubuntu using Docker, securing it with **Nginx** reverse proxy and **Let's Encrypt SSL** via Certbot.

---

### 🔧 Prerequisites

* A running Ubuntu server (we use `192.168.1.165` as example)
* Docker & Docker Compose installed
* Domain from [DuckDNS](https://duckdns.org) (e.g., `garudahostcloud.duckdns.org`)
* Port forwarding configured:

  * External Port 80 → Internal 192.168.1.165:80
  * External Port 443 → Internal 192.168.1.165:443

---

### 1️⃣ Docker Installation (Skip if already done)

```bash
sudo apt update
sudo apt install docker.io docker-compose -y
sudo systemctl enable docker
sudo systemctl start docker
```

---

### 2️⃣ Create Nextcloud Docker Setup

```bash
mkdir ~/nextcloud
cd ~/nextcloud
nano docker-compose.yml
```

**docker-compose.yml**:

```yaml
version: '3'

services:
  db:
    image: mariadb
    restart: always
    command: --transaction-isolation=READ-COMMITTED --binlog-format=ROW
    volumes:
      - db:/var/lib/mysql
    environment:
      - MYSQL_ROOT_PASSWORD=your_root_password
      - MYSQL_PASSWORD=your_password
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud

  app:
    image: nextcloud
    ports:
      - 8080:80
    restart: always
    volumes:
      - nextcloud:/var/www/html
    environment:
      - MYSQL_PASSWORD=your_password
      - MYSQL_DATABASE=nextcloud
      - MYSQL_USER=nextcloud
      - MYSQL_HOST=db

volumes:
  db:
  nextcloud:
```

Start containers:

```bash
docker-compose up -d
```

Test access locally:

```
curl http://localhost:8080
```

---

### 3️⃣ Configure Nginx as Reverse Proxy

Create site config:

```bash
sudo nano /etc/nginx/sites-available/nextcloud
```

Paste:

```nginx
server {
    server_name garudahostcloud.duckdns.org;

    location / {
        proxy_pass http://localhost:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;

        client_max_body_size 1024M;
        proxy_read_timeout 3600;
    }

    listen 443 ssl; # managed by Certbot
    ssl_certificate /etc/letsencrypt/live/garudahostcloud.duckdns.org/fullchain.pem;
    ssl_certificate_key /etc/letsencrypt/live/garudahostcloud.duckdns.org/privkey.pem;
    include /etc/letsencrypt/options-ssl-nginx.conf;
    ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem;
}

server {
    listen 80;
    server_name garudahostcloud.duckdns.org;
    return 301 https://$host$request_uri;
}
```

Enable the site:

```bash
sudo ln -s /etc/nginx/sites-available/nextcloud /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

---

### 4️⃣ SSL Setup with Certbot

```bash
sudo apt install certbot python3-certbot-nginx -y
sudo certbot --nginx -d garudahostcloud.duckdns.org
```

Test renewal:

```bash
sudo certbot renew --dry-run
```

---

### 5️⃣ Update Nextcloud Trusted Domains

```bash
sudo docker exec -it nextcloud-app bash
apt update && apt install nano -y
nano /var/www/html/config/config.php
```

Add to `trusted_domains` array:

```php
  1 => 'garudahostcloud.duckdns.org',
```

Save & Exit.

Restart the container:

```bash
exit
sudo docker restart nextcloud-app
```

---

### ✅ Access Your Nextcloud

Visit:

```
https://garudahostcloud.duckdns.org
```

You can now:

* Upload files
* Share files with public/private links
* Install OnlyOffice or Collabora for document editing
* Sync with desktop/mobile apps

---

### 🚀 Next Steps

* Configure auto backups
* Install apps from Nextcloud App Store
* Enable mobile sync (iOS/Android)
* Enable Nextcloud Office for live document editing

---

You're all set to host your **personal Google Drive alternative**, fully self-managed and secure! 🔐
