# Serenity Docs Customer Setup Guide

Serenity Docs is a PHP and MySQL documentation website with Discord login, license key activation, an admin dashboard, documentation pages, announcements, FAQs, status pages, suggestions, staff applications, optional configs, optional reseller listings, and loader downloads.

This guide is written for customers. Follow the steps in order.

## What You Need

Before installing, make sure your hosting has:

- PHP 8.0 or newer.
- MySQL or MariaDB.
- PHP extensions: `pdo_mysql`, `curl`, `mbstring`, `json`, `fileinfo`.
- HTTPS/SSL enabled.
- Apache with `.htaccess` support, or Nginx with PHP-FPM.
- A Discord Developer application for login.
- A license key from the seller.

Shared hosting is okay if it supports PHP, MySQL, HTTPS, and outbound HTTPS requests.

## Main Files

| File or Folder | What It Is For |
|---|---|
| `setup.sql` | The database setup file you import into MySQL |
| `includes/secrets.local.example.php` | Example settings file |
| `includes/secrets.local.php` | Your private real settings file |
| `.htaccess` | Apache security and rewrite rules |
| `assets/site-images/` | Favicons and branding images |
| `uploads/` | Upload area used by loader/config features |

Do not share your real `includes/secrets.local.php` file.

## Install Checklist

Use this quick checklist if you already know what you are doing:

1. Upload all files to your web root.
2. Create a MySQL database and database user.
3. Import `setup.sql`.
4. Copy `includes/secrets.local.example.php` to `includes/secrets.local.php`.
5. Edit `includes/secrets.local.php`.
6. Create a Discord Developer application.
7. Add your Discord redirect URL.
8. Add your Discord Client ID and Client Secret to `includes/secrets.local.php`.
9. Add your Discord user ID to `ALLOWED_ADMIN_DISCORD_IDS`.
10. Enable HTTPS.
11. Visit `/login.php`.
12. Log in with Discord.
13. Activate your license at `/activate.php`.
14. Open `/admin/dashboard.php`.

## Step 1: Upload The Files

Upload all project files to your website document root.

Common locations:

```text
DirectAdmin: /home/USERNAME/domains/YOURDOMAIN/public_html
cPanel:      /home/USERNAME/public_html
Ubuntu:      /var/www/serenity-docs
```

Important: make sure `.htaccess` uploads too. Some FTP programs hide dotfiles by default.

## Step 2: Create A Database

Create a new MySQL database and database user from your hosting panel.

Write these down:

```text
Database host
Database name
Database username
Database password
```

The database user needs all privileges for that database.

## Step 3: Import The Database

Import this file into your database:

```text
setup.sql
```

In phpMyAdmin:

1. Open phpMyAdmin.
2. Click your database.
3. Click `Import`.
4. Choose `setup.sql`.
5. Click `Go`.
6. Wait for the success message.

Do not rename the tables unless you know what you are doing.

## Step 4: Create Your Settings File

Copy:

```text
includes/secrets.local.example.php
```

To:

```text
includes/secrets.local.php
```

Then edit `includes/secrets.local.php`.

Example:

```php
<?php
define('DB_HOST', 'localhost');
define('DB_USER', 'your_database_user');
define('DB_PASS', 'your_database_password');
define('DB_NAME', 'your_database_name');

define('SITE_NAME', 'Serenity Docs');
define('SITE_URL', 'https://your-domain.com');
define('ADMIN_EMAIL', 'admin@example.com');
define('APP_ENV', 'production');

define('DISCORD_CLIENT_ID', 'your_discord_client_id');
define('DISCORD_CLIENT_SECRET', 'your_discord_client_secret');
define('DISCORD_REDIRECT_URI', SITE_URL . '/auth/discord-callback.php');

define('ALLOWED_ADMIN_DISCORD_IDS', [
    'your_discord_user_id',
]);

define('STORE_URL', 'https://example.com/store');
define('DISCORD_URL', 'https://discord.gg/yourinvite');

define('DISCORD_LOGIN_WEBHOOK_URL', '');
define('DISCORD_VOTE_WEBHOOK_URL', '');
define('DISCORD_CONFIG_WEBHOOK_URL', '');
define('DISCORD_SUGGESTION_WEBHOOK_URL', '');
define('DISCORD_USER_REPORT_WEBHOOK_URL', '');
define('DISCORD_STAFF_APPLICATION_WEBHOOK_URL', '');
```

Important rules:

- `SITE_URL` must use `https://`.
- `SITE_URL` should not end with `/`.
- `DISCORD_REDIRECT_URI` must match the Discord Developer Portal redirect URL exactly.
- Leave webhook URLs blank if you do not use them.
- Keep `APP_ENV` set to `production` unless you are debugging.

## Step 5: Create A Discord Login App

The site uses Discord login.

1. Go to the Discord Developer Portal.
2. Click `New Application`.
3. Give it a name.
4. Open `OAuth2`.
5. Copy the Client ID.
6. Copy or reset the Client Secret.
7. Add this redirect URL:

```text
https://your-domain.com/auth/discord-callback.php
```

8. Save changes in Discord.
9. Put the Client ID and Client Secret into `includes/secrets.local.php`.

## Step 6: Add Yourself As Admin

You need your Discord user ID.

1. Open Discord.
2. Open User Settings.
3. Go to Advanced.
4. Enable Developer Mode.
5. Right-click your Discord profile.
6. Click `Copy User ID`.
7. Add it to `ALLOWED_ADMIN_DISCORD_IDS`.

Example:

```php
define('ALLOWED_ADMIN_DISCORD_IDS', [
    '123456789012345678',
]);
```

If you need multiple admins:

```php
define('ALLOWED_ADMIN_DISCORD_IDS', [
    '123456789012345678',
    '987654321098765432',
]);
```

## Step 7: Set Permissions

Most hosts work with:

```text
Folders: 755
Files:   644
```

These folders may need write access:

```text
uploads/
uploads/loader/
```

Try `755` first. If uploads fail, try `775` for upload folders only.

Avoid `777` unless your host specifically requires it.

## Step 8: Log In

Open:

```text
https://your-domain.com/login.php
```

Log in with Discord.

If Discord sends you to an error page, check your redirect URL and Discord settings.

## Step 9: Activate Your License

Open:

```text
https://your-domain.com/activate.php
```

Paste the license key you were given by the seller.

After activation, you should be able to access the protected site pages.

## Step 10: Open The Admin Dashboard

Open:

```text
https://your-domain.com/admin/dashboard.php
```

If you are redirected away, your Discord user ID is probably missing or incorrect in `ALLOWED_ADMIN_DISCORD_IDS`.

## Admin Pages

| Page | What It Does |
|---|---|
| `admin/dashboard.php` | Main dashboard and site settings |
| `admin/add_doc.php` | Add documentation pages |
| `admin/edit_doc.php` | Edit existing documentation |
| `admin/configs_manage.php` | Approve, deny, delete, and enable public configs |
| `admin/resellers_manage.php` | Manage reseller listings and public reseller visibility |
| `admin/announcements_manage.php` | Add banners and popups |
| `admin/suggestions_manage.php` | Review public suggestions |
| `admin/staff_applications_manage.php` | Review support staff applications |
| `admin/status.php` | Manage service status |
| `admin/faq_manage.php` | Manage FAQ entries |
| `admin/accounts_manage.php` | Manage users and bans |

## Default Public Features

Some optional pages are hidden by default on fresh installs.

| Feature | Default |
|---|---|
| Configs page | Hidden |
| Resellers nav link | Hidden |
| Resellers homepage block | Hidden |
| Staff application page | Available by direct link |
| Suggestions page | Available |
| Status page | Available |
| FAQ page | Available |

Enable Configs:

```text
Admin Dashboard -> Site Settings -> Show Configs on the public site
```

Or:

```text
Admin -> Manage -> Config management -> Public Configs Page
```

Enable Resellers:

```text
Admin -> Manage -> Reseller listings -> Public site
```

The staff application public link is shown here:

```text
Admin -> Manage -> Staff applications
```

The public staff application URL is:

```text
https://your-domain.com/apps.php
```

## Adding Documentation

To add a page:

1. Open `admin/dashboard.php`.
2. Click or use `Add Documentation Page`.
3. Enter a title.
4. Enter the content.
5. Save.

Documentation content supports Markdown.

Example Markdown:

```markdown
# Main Title

## Section Title

- Bullet point
- Another point

**Bold text**

`inline code`
```

## Announcements

Announcements can be used for:

- Important updates.
- Maintenance notices.
- Warnings.
- Homepage banners.
- Popups.

Open:

```text
Admin -> Manage -> Announcements
```

## FAQ

Open:

```text
Admin -> Manage -> FAQ management
```

Use FAQs for common customer questions.

## Status Page

Open:

```text
Admin -> Manage -> Status management
```

Use this page to show service status, uptime notes, or maintenance information.

## Suggestions

Users can submit suggestions from:

```text
https://your-domain.com/suggestions.php
```

Admins review them from:

```text
Admin -> Manage -> Suggestions
```

## Staff Applications

The public staff application page is:

```text
https://your-domain.com/apps.php
```

Admins review applications from:

```text
Admin -> Manage -> Staff applications
```

That admin page also shows the public application link so you can copy it.

## Configs

Configs are hidden by default.

To enable them:

```text
Admin -> Manage -> Config management -> Public Configs Page
```

When enabled, users can browse and submit configs.

Admins can approve or deny configs before they appear publicly.

## Resellers

Resellers are hidden by default.

To enable them:

```text
Admin -> Manage -> Reseller listings -> Public site
```

You can choose whether resellers show in the navigation and on the homepage.

## Loader Download

You can show a `Download Loader` button in the navigation.

### External Loader URL

Open:

```text
Admin Dashboard -> Site Settings
```

Set:

```text
Loader Download URL
```

Use a full public URL.

Example:

```text
https://example.com/downloads/loader.exe
```

### Local Loader Files

Loader files can be placed in:

```text
uploads/loader/
```

The protected loader endpoint is:

```text
https://your-domain.com/loader/update
```

Do not link users directly to files inside `uploads/loader/`.

## Branding And Favicons

Brand images live in:

```text
assets/site-images/
```

Replace these files with your own versions, keeping the same filenames:

| File | Purpose |
|---|---|
| `favicon.ico` | Main browser favicon |
| `favicon-16x16.png` | Small favicon |
| `favicon-32x32.png` | Standard favicon |
| `favicon-48x48.png` | Larger favicon |
| `apple-touch-icon.png` | Apple touch icon |
| `android-chrome-192x192.png` | Android/PWA icon |
| `android-chrome-512x512.png` | Android/PWA icon |
| `site.webmanifest` | Web app manifest |
| `og-default.png` | Optional social preview image |

Also update these files with your real domain/name if needed:

```text
site.webmanifest
robots.txt
sitemap.xml
```

## Optional Discord Webhooks

Webhook settings are in:

```text
includes/secrets.local.php
```

Leave a webhook blank to disable it:

```php
define('DISCORD_LOGIN_WEBHOOK_URL', '');
define('DISCORD_VOTE_WEBHOOK_URL', '');
define('DISCORD_CONFIG_WEBHOOK_URL', '');
define('DISCORD_SUGGESTION_WEBHOOK_URL', '');
define('DISCORD_USER_REPORT_WEBHOOK_URL', '');
define('DISCORD_STAFF_APPLICATION_WEBHOOK_URL', '');
```

If enabled, the site can send Discord notifications for logins, votes, config submissions, suggestions, reports, and staff applications.

## Choose Your Install Method

GitHub README files do not support real tabs, so use the expandable section that matches your hosting.

Before choosing a method, make sure your domain DNS is pointed at your server or hosting account:

```text
A record:    your-domain.com -> your server IP
CNAME:       www -> your-domain.com
```

DNS can take a few minutes to several hours to update. SSL will usually fail until DNS points to the correct server.

<details>
<summary><strong>Method 1: DirectAdmin Shared Hosting</strong></summary>

Use this if your host gives you a DirectAdmin panel.

### 1. Set The PHP Version

1. Log into DirectAdmin.
2. Open `Domain Setup`.
3. Select your domain.
4. Set PHP to `8.0` or newer.
5. Save.

Some DirectAdmin panels call this `PHP Version Selector` or `Select PHP Version`.

### 2. Create The Database

1. Open `MySQL Management`.
2. Click `Create New Database`.
3. Create a database name.
4. Create a database user.
5. Use a strong password.
6. Save the database name, username, and password.

Your database host is usually:

```text
localhost
```

### 3. Import The SQL

1. Open phpMyAdmin from DirectAdmin.
2. Click your new database on the left.
3. Click `Import`.
4. Select `setup.sql`.
5. Click `Go`.
6. Wait for a success message.

### 4. Upload The Files

Upload the project files to:

```text
/domains/YOURDOMAIN/public_html
```

Make sure `.htaccess` uploads. If you do not see `.htaccess`, enable hidden files in your FTP client or File Manager.

### 5. Create Your Settings File

Copy:

```text
includes/secrets.local.example.php
```

To:

```text
includes/secrets.local.php
```

Edit `includes/secrets.local.php` and set:

```php
define('DB_HOST', 'localhost');
define('DB_USER', 'your_database_user');
define('DB_PASS', 'your_database_password');
define('DB_NAME', 'your_database_name');
define('SITE_URL', 'https://your-domain.com');
```

Then add your Discord Client ID, Client Secret, redirect URI, and admin Discord ID.

### 6. Enable SSL

In DirectAdmin, open:

```text
SSL Certificates -> Free & automatic certificate from Let's Encrypt
```

Select your domain and `www` if you use it, then issue the certificate.

### 7. Test

Open:

```text
https://your-domain.com/login.php
```

Then log in with Discord, activate your license, and open:

```text
https://your-domain.com/admin/dashboard.php
```

</details>

<details>
<summary><strong>Method 2: cPanel Shared Hosting</strong></summary>

Use this if your host gives you cPanel.

### 1. Set The PHP Version

1. Log into cPanel.
2. Open `MultiPHP Manager`.
3. Select your domain.
4. Choose PHP `8.0` or newer.
5. Apply.

If your cPanel has `Select PHP Version`, use that instead and enable:

```text
pdo_mysql
curl
mbstring
json
fileinfo
```

### 2. Create The Database

1. Open `MySQL Databases`.
2. Create a new database.
3. Create a database user.
4. Add the user to the database.
5. Give the user `ALL PRIVILEGES`.
6. Save the database name, username, and password.

Database host is usually:

```text
localhost
```

### 3. Import The SQL

1. Open phpMyAdmin.
2. Select your database.
3. Click `Import`.
4. Select `setup.sql`.
5. Click `Go`.

### 4. Upload The Files

For your main domain, upload to:

```text
public_html
```

For an addon domain, upload to the document root shown in cPanel for that addon domain.

Make sure `.htaccess` uploads.

### 5. Create Your Settings File

Copy:

```text
includes/secrets.local.example.php
```

To:

```text
includes/secrets.local.php
```

Edit `includes/secrets.local.php` with your database, site URL, Discord app, and admin Discord ID.

### 6. Enable SSL

Open:

```text
SSL/TLS Status
```

Run AutoSSL if the domain is not already secured.

### 7. Test

Open:

```text
https://your-domain.com/login.php
```

If the site shows a 500 error, check that PHP is 8.0+ and all required PHP extensions are enabled.

</details>

<details>
<summary><strong>Method 3: Ubuntu VPS With Apache</strong></summary>

Use this if you have a fresh Ubuntu VPS and want Apache to serve the site directly.

### 1. Point DNS To The Server

At your domain/DNS provider, create:

```text
A record: your-domain.com -> YOUR_SERVER_IP
CNAME:    www -> your-domain.com
```

Wait for DNS to update before issuing SSL.

### 2. Install Packages

```bash
sudo apt update
sudo apt install apache2 mysql-server php php-mysql php-curl php-mbstring php-json php-fileinfo libapache2-mod-php unzip certbot python3-certbot-apache
```

Enable required Apache modules:

```bash
sudo a2enmod rewrite headers expires deflate ssl
sudo systemctl restart apache2
```

### 3. Create The Site Folder

```bash
sudo mkdir -p /var/www/serenity-docs
sudo chown -R $USER:www-data /var/www/serenity-docs
```

Upload or unzip the site files into:

```text
/var/www/serenity-docs
```

If you uploaded a zip:

```bash
cd /var/www/serenity-docs
unzip serenity-docs.zip
```

The files should be directly inside `/var/www/serenity-docs`, not inside an extra nested folder.

### 4. Create The MySQL Database

Log into MySQL:

```bash
sudo mysql
```

Run these commands. Change the database name, username, and password:

```sql
CREATE DATABASE serenity_docs CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'serenity_user'@'localhost' IDENTIFIED BY 'CHANGE_THIS_PASSWORD';
GRANT ALL PRIVILEGES ON serenity_docs.* TO 'serenity_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Import the SQL:

```bash
mysql -u serenity_user -p serenity_docs < /var/www/serenity-docs/setup.sql
```

### 5. Create Your Settings File

```bash
cd /var/www/serenity-docs
cp includes/secrets.local.example.php includes/secrets.local.php
nano includes/secrets.local.php
```

Set:

```php
define('DB_HOST', 'localhost');
define('DB_USER', 'serenity_user');
define('DB_PASS', 'CHANGE_THIS_PASSWORD');
define('DB_NAME', 'serenity_docs');
define('SITE_URL', 'https://your-domain.com');
```

Also set your Discord Client ID, Client Secret, redirect URI, and admin Discord ID.

Save in nano:

```text
Ctrl + O
Enter
Ctrl + X
```

### 6. Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/serenity-docs
sudo find /var/www/serenity-docs -type d -exec chmod 755 {} \;
sudo find /var/www/serenity-docs -type f -exec chmod 644 {} \;
sudo chmod -R 775 /var/www/serenity-docs/uploads
```

### 7. Create The Apache Virtual Host

Create:

```bash
sudo nano /etc/apache2/sites-available/serenity-docs.conf
```

Paste this:

```apache
<VirtualHost *:80>
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    DocumentRoot /var/www/serenity-docs

    <Directory /var/www/serenity-docs>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/serenity-docs-error.log
    CustomLog ${APACHE_LOG_DIR}/serenity-docs-access.log combined
</VirtualHost>
```

Enable the site:

```bash
sudo a2dissite 000-default.conf
sudo a2ensite serenity-docs.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
```

### 8. Add SSL

```bash
sudo certbot --apache -d your-domain.com -d www.your-domain.com
```

Choose the redirect-to-HTTPS option when Certbot asks.

### 9. Test

Open:

```text
https://your-domain.com/login.php
```

Useful log commands:

```bash
sudo tail -f /var/log/apache2/serenity-docs-error.log
sudo tail -f /var/log/apache2/error.log
```

</details>

<details>
<summary><strong>Method 4: Ubuntu VPS With Nginx + PHP-FPM</strong></summary>

Use this if you want Nginx to serve the site directly.

### 1. Point DNS To The Server

At your domain/DNS provider:

```text
A record: your-domain.com -> YOUR_SERVER_IP
CNAME:    www -> your-domain.com
```

### 2. Install Packages

```bash
sudo apt update
sudo apt install nginx mysql-server php-fpm php-mysql php-curl php-mbstring php-json php-fileinfo unzip certbot python3-certbot-nginx
```

Find your PHP-FPM socket:

```bash
ls /run/php/
```

Example output:

```text
php8.3-fpm.sock
```

Use whatever socket your server shows.

### 3. Create The Site Folder

```bash
sudo mkdir -p /var/www/serenity-docs
sudo chown -R $USER:www-data /var/www/serenity-docs
```

Upload or unzip the project into:

```text
/var/www/serenity-docs
```

### 4. Create And Import The Database

```bash
sudo mysql
```

```sql
CREATE DATABASE serenity_docs CHARACTER SET utf8mb4 COLLATE utf8mb4_unicode_ci;
CREATE USER 'serenity_user'@'localhost' IDENTIFIED BY 'CHANGE_THIS_PASSWORD';
GRANT ALL PRIVILEGES ON serenity_docs.* TO 'serenity_user'@'localhost';
FLUSH PRIVILEGES;
EXIT;
```

Import:

```bash
mysql -u serenity_user -p serenity_docs < /var/www/serenity-docs/setup.sql
```

### 5. Create The Settings File

```bash
cd /var/www/serenity-docs
cp includes/secrets.local.example.php includes/secrets.local.php
nano includes/secrets.local.php
```

Set database, site URL, Discord, and admin Discord ID values.

### 6. Set Permissions

```bash
sudo chown -R www-data:www-data /var/www/serenity-docs
sudo find /var/www/serenity-docs -type d -exec chmod 755 {} \;
sudo find /var/www/serenity-docs -type f -exec chmod 644 {} \;
sudo chmod -R 775 /var/www/serenity-docs/uploads
```

### 7. Create The Nginx Site Config

Create:

```bash
sudo nano /etc/nginx/sites-available/serenity-docs
```

Paste this. Change `php8.3-fpm.sock` if your server showed a different socket:

```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;
    root /var/www/serenity-docs;
    index index.php index.html;

    client_max_body_size 100m;

    location / {
        try_files $uri $uri/ /index.php?$query_string;
    }

    location = /loader/update {
        rewrite ^ /loader/update.php last;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.3-fpm.sock;
    }

    location ~ /\.(?!well-known) {
        deny all;
    }

    location ~* \.(sql|bak|backup|old|orig|save|swp|tmp)$ {
        deny all;
    }

    location ~* ^/(config\.php|includes/secrets\.local\.php|includes/runtime\.php)$ {
        deny all;
    }

    location ^~ /uploads/loader/ {
        deny all;
    }
}
```

Enable it:

```bash
sudo ln -s /etc/nginx/sites-available/serenity-docs /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```

### 8. Add SSL

```bash
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```

Choose redirect to HTTPS if prompted.

### 9. Test

Open:

```text
https://your-domain.com/login.php
```

Useful logs:

```bash
sudo tail -f /var/log/nginx/error.log
sudo journalctl -u php*-fpm -f
```

</details>

<details>
<summary><strong>Method 5: Ubuntu Nginx Reverse Proxy To Apache</strong></summary>

Use this if you want Nginx to handle the public domain and SSL, but Apache to run the PHP site behind it.

This is useful when:

- You already use Apache configs.
- You want Nginx in front for SSL/proxy handling.
- You host multiple apps and want Nginx as the main entry point.

### 1. DNS

Point your domain to the server:

```text
A record: your-domain.com -> YOUR_SERVER_IP
CNAME:    www -> your-domain.com
```

### 2. Install Packages

```bash
sudo apt update
sudo apt install nginx apache2 mysql-server php php-mysql php-curl php-mbstring php-json php-fileinfo libapache2-mod-php unzip certbot python3-certbot-nginx
```

Enable Apache modules:

```bash
sudo a2enmod rewrite headers
sudo systemctl restart apache2
```

### 3. Move Apache To Port 8080

Edit Apache ports:

```bash
sudo nano /etc/apache2/ports.conf
```

Change:

```apache
Listen 80
```

To:

```apache
Listen 127.0.0.1:8080
```

### 4. Create The Site Folder

```bash
sudo mkdir -p /var/www/serenity-docs
sudo chown -R $USER:www-data /var/www/serenity-docs
```

Upload files to:

```text
/var/www/serenity-docs
```

Create `includes/secrets.local.php`, import `setup.sql`, and set permissions the same way as the Apache method above.

### 5. Create The Apache Backend Site

Create:

```bash
sudo nano /etc/apache2/sites-available/serenity-docs-backend.conf
```

Paste:

```apache
<VirtualHost 127.0.0.1:8080>
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    DocumentRoot /var/www/serenity-docs

    <Directory /var/www/serenity-docs>
        AllowOverride All
        Require all granted
    </Directory>

    ErrorLog ${APACHE_LOG_DIR}/serenity-docs-backend-error.log
    CustomLog ${APACHE_LOG_DIR}/serenity-docs-backend-access.log combined
</VirtualHost>
```

Enable Apache backend:

```bash
sudo a2dissite 000-default.conf
sudo a2ensite serenity-docs-backend.conf
sudo apache2ctl configtest
sudo systemctl reload apache2
```

### 6. Create The Nginx Public Proxy

Create:

```bash
sudo nano /etc/nginx/sites-available/serenity-docs
```

Paste:

```nginx
server {
    listen 80;
    server_name your-domain.com www.your-domain.com;

    client_max_body_size 100m;

    location / {
        proxy_pass http://127.0.0.1:8080;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enable Nginx site:

```bash
sudo ln -s /etc/nginx/sites-available/serenity-docs /etc/nginx/sites-enabled/
sudo rm -f /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```

### 7. Add SSL On Nginx

```bash
sudo certbot --nginx -d your-domain.com -d www.your-domain.com
```

Nginx will accept HTTPS traffic and proxy it to Apache on `127.0.0.1:8080`.

### 8. Test The Proxy

Check Apache locally:

```bash
curl -I http://127.0.0.1:8080
```

Check the public domain:

```bash
curl -I https://your-domain.com
```

Useful logs:

```bash
sudo tail -f /var/log/nginx/error.log
sudo tail -f /var/log/apache2/serenity-docs-backend-error.log
```

</details>

## Security Checklist

Before going live:

- Use HTTPS.
- Keep `includes/secrets.local.php` private.
- Do not upload database backups publicly.
- Do not share Discord Client Secrets.
- Do not share webhook URLs.
- Keep `APP_ENV` set to `production`.
- Make sure `.htaccess` uploaded on Apache hosting.
- Make sure `.sql`, `.bak`, `.old`, `.tmp`, and hidden files are blocked from the browser.
- Keep database backups somewhere private.

## Files You Should Not Share Publicly

Do not upload these to a public GitHub repo or public download:

```text
includes/secrets.local.php
*.sql database backups
*.bak
*.backup
*.old
*.tmp
.env
```

The included `setup.sql` file is okay to use for installation. Private database exports/backups are not.

## Troubleshooting

### White Page Or HTTP 500

Check:

- PHP version is 8.0 or newer.
- `includes/secrets.local.php` exists.
- Database credentials are correct.
- `setup.sql` was imported.
- PHP extensions are enabled.
- File permissions are correct.
- Apache has `AllowOverride All`.
- `.htaccess` uploaded correctly.

To debug temporarily, set this in `includes/secrets.local.php`:

```php
define('APP_ENV', 'development');
```

Switch it back after debugging:

```php
define('APP_ENV', 'production');
```

### Discord Login Redirects Wrong

The Discord redirect must exactly match:

```text
https://your-domain.com/auth/discord-callback.php
```

Also check:

```php
define('SITE_URL', 'https://your-domain.com');
define('DISCORD_REDIRECT_URI', SITE_URL . '/auth/discord-callback.php');
```

Common mistakes:

- Using `http://` instead of `https://`.
- Adding a trailing slash to `SITE_URL`.
- Typing the wrong domain.
- Forgetting to save the redirect URL in Discord Developer Portal.

### Admin Dashboard Sends You Away

Your Discord user ID is not listed as an admin.

Fix:

```php
define('ALLOWED_ADMIN_DISCORD_IDS', [
    'your_real_discord_user_id',
]);
```

Then log out and log back in.

### License Key Says Not Found

Check:

- You copied the key correctly.
- There are no extra spaces before or after the key.
- You are using the key on the correct site.
- The key has not already reached its activation limit.
- The key has not expired.
- Your server can make outbound HTTPS requests.

If it still fails, contact the seller with the exact error message.

Do not post your license key publicly.

### Configs Or Resellers Are Missing

They are hidden by default.

Enable Configs:

```text
Admin Dashboard -> Site Settings -> Show Configs on the public site
```

Enable Resellers:

```text
Admin -> Manage -> Reseller listings -> Public site
```

### Staff Application Link

The public link is:

```text
https://your-domain.com/apps.php
```

The admin page shows the link here:

```text
Admin -> Manage -> Staff applications
```

### Uploads Fail

Check permissions for:

```text
uploads/
uploads/loader/
```

Try:

```text
755 first
775 if needed
```

Avoid `777` unless your host specifically requires it.

### CSS Or Icons Look Old

Clear browser cache or hard refresh:

```text
Windows: Ctrl + F5
Mac:     Cmd + Shift + R
```

Also clear any hosting cache or CDN cache.

## Final Launch Checklist

Before sharing the site with users:

1. Homepage loads.
2. Discord login works.
3. License activation works.
4. Admin dashboard opens for your Discord account.
5. Documentation pages load.
6. FAQ page loads.
7. Status page loads.
8. Suggestions page loads.
9. Staff application link opens.
10. Configs are enabled or hidden as you prefer.
11. Resellers are enabled or hidden as you prefer.
12. Loader download button works if you use it.
13. `includes/secrets.local.php` does not open in the browser.
14. HTTPS is active.

## Getting Help

When asking for help, include:

- Hosting type: DirectAdmin, cPanel, Apache VPS, Nginx VPS, or other.
- PHP version.
- MySQL or MariaDB version.
- Exact URL that fails.
- Screenshot or full error message.
- Whether the issue happens before or after login.
- Whether `APP_ENV` is `production` or `development`.

Do not send real passwords, Discord Client Secrets, webhook URLs, or license keys in public support messages.
