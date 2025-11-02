# Royal's CCW Portfolio Deployment

## Project Description
A sample portfolio website deployed on a live Apache server using AWS EC2. This project demonstrates hands-on experience with cloud hosting, Linux permissions, SCP file transfer, and Apache configuration.

## Live Preview
[View Live Portfolio](http://51.21.134.96/portfolio_apache/)

## Tech Stack
- HTML/CSS
- Apache2
- Ubuntu (EC2)
- SCP (Secure Copy Protocol)
- AWS EC2

## Deployment Steps

### 1. Launch Ubuntu EC2 Instance on AWS
- Create a new EC2 instance with Ubuntu as the operating system.

### 2. Configure Security Group
- Allow HTTP traffic on port 80.

### 3. Install Apache2
```bash
sudo apt update && sudo apt install apache2
```

### 4. Confirm Apache Installation
- Visit `http://<public-ip>` to verify Apache is running.

### 5. Transfer Files Using SCP
```bash
scp -i "path/to/key.pem" -r "local/path/to/portfolio_apache" ubuntu@<public-ip>:~/portfolio_apache
```

### 6. Move Files to Apache Root Directory
```bash
sudo mv ~/portfolio_apache/* /var/www/html/
```

### 7. Set Correct Permissions
```bash
sudo chown -R www-data:www-data /var/www/html/
sudo chmod -R 755 /var/www/html/
```

### 8. Restart Apache
```bash
sudo systemctl restart apache2
```

## Apache Notes
- Apache serves files from `/var/www/html/` by default.
- Default page is `index.html`.
- Folder permissions must allow read access for `www-data` user/group.
- Use `ls -l` to inspect file ownership and permissions.
- Avoid placing files in root-owned directories without proper sudo access.
- **Configuration Files**: Main config is `/etc/apache2/apache2.conf`. Site-specific configs in `/etc/apache2/sites-available/`.
- **Virtual Hosts**: Enable multiple sites on one server using `<VirtualHost>` directives. Example: Create a new config file in `sites-available`, enable with `a2ensite`, and reload Apache.
- **Modules**: Enable/disable modules with `a2enmod`/`a2dismod` (e.g., `a2enmod rewrite` for URL rewriting).
- **Logs**: Check `/var/log/apache2/error.log` and `access.log` for issues.
- **Security**: Use `.htaccess` for directory-level configs, but prefer server config for performance.

## Deployment Guides

### Static Sites (HTML/CSS/JS)
#### Ubuntu (Apache2)
1. Install Apache2: `sudo apt update && sudo apt install apache2`
2. Transfer files to `/var/www/html/`
3. Set permissions: `sudo chown -R www-data:www-data /var/www/html/ && sudo chmod -R 755 /var/www/html/`
4. Restart Apache: `sudo systemctl restart apache2`

#### Windows (IIS)
1. Install IIS via Windows Features.
2. Place files in `C:\inetpub\wwwroot\`
3. Ensure IIS_IUSRS has read access.
4. Restart IIS from Services.

### Node.js Apps
#### Ubuntu (Apache2 with Proxy)
1. Install Node.js: `curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash - && sudo apt-get install -y nodejs`
2. Install PM2: `sudo npm install -g pm2`
3. Run app: `pm2 start app.js --name myapp`
4. Configure Apache proxy: Edit `/etc/apache2/sites-available/000-default.conf` to add `ProxyPass / http://localhost:3000/`
5. Enable proxy modules: `sudo a2enmod proxy proxy_http`
6. Restart Apache: `sudo systemctl restart apache2`

#### Windows (IIS with iisnode)
1. Install iisnode module for IIS.
2. Configure web.config for Node.js app.
3. Place app in IIS site directory.
4. Restart IIS.

### Python Flask/Django Apps
#### Ubuntu (Apache2 with WSGI)
1. Install Python and pip: `sudo apt install python3 python3-pip`
2. Install mod_wsgi: `sudo apt install libapache2-mod-wsgi-py3`
3. For Flask: Create wsgi.py, configure Apache with WSGIScriptAlias.
4. For Django: Use gunicorn or uwsgi, proxy via Apache.
5. Set permissions and restart Apache.

#### Windows (IIS with wfastcgi)
1. Install wfastcgi: `pip install wfastcgi`
2. Enable wfastcgi: `wfastcgi --enable`
3. Configure web.config in IIS site.
4. Restart IIS.

## Troubleshooting
- **“403 Forbidden”** → Check file permissions and ownership.
- **“Site not reachable”** → Ensure EC2 is running and security group allows port 80.
- **SCP errors** → Verify `.pem` path and use Git Bash or PowerShell.

## Author
Royal from Chichawatni (CCW), Pakistan  
Passionate about modular design, cloud deployment, and blending local identity with global tech.

## License
MIT License
