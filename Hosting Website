## 🌐 Host a Static Website with Nginx

This section details the steps to install and configure Nginx on Ubuntu Server to serve a static website, both on the local network and publicly over the internet using DuckDNS and HTTPS.

### 1. **Install Nginx Web Server**

Nginx is a high-performance web server often used to serve static websites.

```bash
sudo apt update
sudo apt install nginx -y
```

After installation, ensure the service is running:

```bash
sudo systemctl status nginx
```

You should see `active (running)`.

### 2. **Allow Nginx Through UFW Firewall**

Allow web traffic on HTTP (port 80) and HTTPS (port 443):

```bash
sudo ufw allow 'Nginx Full'
sudo ufw reload
```

### 3. **Verify the Web Server**

Visit the server's local IP in a browser:

```
http://192.168.1.95
```

You should see the default Nginx welcome page.

### 4. **Replace the Default Index Page with Custom HTML**

Create or replace the default homepage with your custom static page:

```bash
echo "<h1>Hello from Vishnu's Home Server 🚀</h1>" | sudo tee /var/www/html/index.html
```

Or copy your own HTML files:

```bash
sudo rm -rf /var/www/html/*
sudo cp ~/my-website/* /var/www/html/
sudo systemctl reload nginx
```

### 5. **Set Correct Permissions**

Ensure the server user owns the web root to allow easy editing:

```bash
sudo chown -R $USER:$USER /var/www/html
```

### 6. **Test Locally**

Again visit:

```
http://192.168.1.95
```

You should now see your custom website.

### 7. **Optional: Create a Simple HTML Page**

You can create a file manually:

```bash
nano ~/index.html
```

Paste your HTML content, then copy it to the web root:

```bash
sudo mv ~/index.html /var/www/html/index.html
```

### 8. **Check Nginx Logs for Errors**

If needed, use the logs to debug:

```bash
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

---
