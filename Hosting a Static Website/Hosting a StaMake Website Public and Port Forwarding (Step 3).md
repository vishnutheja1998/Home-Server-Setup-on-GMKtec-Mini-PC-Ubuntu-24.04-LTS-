
## 🌍 Make the Website Public Using DuckDNS + Port Forwarding

### 9. **Register a Free Domain on DuckDNS**

1. Visit [https://www.duckdns.org](https://www.duckdns.org)
2. Sign in with GitHub or Google
3. Register a domain (e.g., `garudahost.duckdns.org`)
4. Note the token given to you — it will be used in the update script

### 10. **Set Up DuckDNS Update Script**

Create a directory for DuckDNS updater:

```bash
sudo mkdir -p /opt/duckdns
cd /opt/duckdns
```

Create the script:

```bash
sudo nano duck.sh
```

Add the following content (replace with your domain and token):

```bash
echo url="https://www.duckdns.org/update?domains=garudahost&token=YOUR_TOKEN&ip=" | curl -k -o /opt/duckdns/duck.log -K -
```

Make it executable:

```bash
sudo chmod 700 duck.sh
```

### 11. **Add Cron Job for Periodic Updates**

```bash
crontab -e
```

Add this line:

```bash
*/5 * * * * /opt/duckdns/duck.sh >/dev/null 2>&1
```

This will update your public IP with DuckDNS every 5 minutes.

### 12. **Set Up Port Forwarding in Your Router**

1. Visit your router IP (e.g., `http://192.168.1.254`)
2. Login and find the Port Forwarding or NAT/Gaming section
3. Forward these ports:

   * Port **80 (HTTP)** → **192.168.1.95** (your server's internal IP)
   * Port **443 (HTTPS)** → **192.168.1.95**
4. Save and apply changes

Now your server is reachable publicly:

```
http://garudahost.duckdns.org
```

---
