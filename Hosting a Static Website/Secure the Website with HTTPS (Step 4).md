
## 🔐 Secure the Website with HTTPS (Let's Encrypt + Certbot)

### 13. **Install Certbot for Nginx**

```bash
sudo apt install certbot python3-certbot-nginx -y
```

### 14. **Request an SSL Certificate**

```bash
sudo certbot --nginx -d garudahost.duckdns.org
```

Follow the prompts:

* Agree to terms
* Optionally provide your email
* Choose to redirect HTTP to HTTPS (recommended)

### 15. **Test Renewal Process**

```bash
sudo certbot renew --dry-run
```

You can now access your site securely:

```
https://garudahost.duckdns.org
```

---

With this setup, your home server is now:

* Hosting a public static website
* Protected with HTTPS
* Automatically updating its public IP

You're ready to expand with Docker, Nextcloud, or even a blog platform like Hugo or Ghost!

---
