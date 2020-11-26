# Graphical User Interfaces
---
## Web Server

#### Nginx

###### Site Config Files
- Test config files (and show location: `nginx -t`
- Location: `/etc/nginx/sites-enabled/*.conf`
- enable site: `ln -s /etc/nginx/sites-available/www.example.org.conf /etc/nginx/sites-enabled/`
- reload apache service: `service nginx reload`

#### Apache
- check status: `systemctl status apache2`

###### Redirect HTTP to HTTPS vhost
```
ServerAdmin dev@designtennis.com
ServerName nedic.ca
ServerAlias *.nedic.ca, www.nedic.ca
Redirect 301 / https://nedic.ca/
```

###### HTTPS vhost config

```
    ServerAdmin dev@designtennis.com
	ServerName nedic.ca
	ServerAlias *.nedic.ca, www.nedic.ca
    DocumentRoot /srv/www/nedic.ca/public_html/
```
	<Directory /srv/www/nedic.ca/public_html>
	Options +ExecCGI
	Require all granted
	</Directory>

    ErrorLog ${APACHE_LOG_DIR}/nedic.ca-error.log
	CustomLog ${APACHE_LOG_DIR}/nedic.ca-access.log combined


    Alias /static /srv/www/nedic.ca/public_html/static
	Alias /media /srv/www/nedic.ca/public_html/media
	SetOutputFilter DEFLATE
	AddOutputFilterByType DEFLATE text/html text/plain text/xml text/css application/rss+xml application/x-javascript application/atom_xml application/javascript
	WSGIDaemonProcess nedic.ca python-path=/srv/www/nedic.ca/env/lib/python3.6/site-packages:/srv/www/nedic.ca/application  processes=2 threads=15 display-name=%{edit}
	WSGIProcessGroup nedic.ca
	WSGIScriptAlias / /srv/www/nedic.ca/public_html/wsgi.py


###### Site Config Files
- Location: `/etc/apache2/sites-available`
- enable site: `a2ensite {config file}`
- disable site: `a2dissite {config file}`
- reload apache service: `systemctl reload` apache2`

###### Canonical Domain (URI)
	RewriteEngine On
	RewriteCond %{HTTP_HOST} ^www\.nedic\.ca$ [NC]
	RewriteRule (.*) https://nedic.ca$1 [L,R=301]

---
## SSL

##### Let's Encrypt
###### Install certbot

- `sudo add-apt-repository ppa:certbot/certbot`
- `sudo apt-get update`
- `sudo apt-get install certbot`

###### Renew Certificate
- should run automatically
    - Ubuntu uses cronjob in `/etc/cron.d/cerbot`
- manually: `certbot renew`
- expand (include subdomain): `ertbot -d {example.com,www.example.com} --expand`

###### View Certificates
- `certbot certificates`

##### Automatically generated ssl config
	Include /etc/letsencrypt/options-ssl-apache.conf
	SSLCertificateFile /etc/letsencrypt/live/nedic.ca/fullchain.pem
	SSLCertificateKeyFile /etc/letsencrypt/live/nedic.ca/privkey.pem

---
## Jinja
#### Registering custom filters
```
from django_jinja import library
@library.filter
def mylower(name):
"""
Usage: {{ 'Hello'|mylower() }}
"""
return name.lower()
```

#### Alertnative to register custom filter in Django settings.py
```
TEMPLATES = [
{
    "BACKEND": "django_jinja.backend.Jinja2",
    "APP_DIRS": True,
    "OPTIONS": {
        "match_extension": ".jinja",
        "filters": {
            "myfilter": "path.to.filters.myfilterfn",
            ...
        }
    }
}]
```

---
## Flask


---
## Django



---
## WordPress
export db: `wp db export`
import db: `wp db import`

#### XAMPP
- "env: mysql: No such file or directory after `wp import`"
	- export mysql to path: `export PATH=$PATH:/Applications/XAMPP/xamppfiles/bin`
- "XAMPP (Mac) Virtual host showing 403"
	- "To fix this you can configure Apache to run as your OS X user. Open /Applications/XAMPP/xamppfiles/etc/httpd.conf and look for the following lines:"
	- User/Group: The name (or #number) of the user/group to run httpd as. It is usually good practice to create a dedicated user and group for running httpd, as with most system services.
	- `User daemon`
	- `Group daemon`
	- Change User to your OS X username or add new user as follow, and save the file:
	- `User yourusername`


---
## Vue


---
## JavaScript
install node and npm: `brew install nvm`
list versions: `nvm list`
install node version (comes with specific npm version): `nvm install [version]`
show current version: `nvm current`
use a different version: `nvm use [version]`

---
## CSS


---
## HTML


---
## Resources
- [Vue UI Libarry](https://vuetifyjs.com/en/)
- HTML Tempaltes
    - [html5up](https://html5up.net/)
    - [templated](https://templated.co/)

---
## Notes
Block entire subdomain from getting indexed by search engines: robot.txt in root dir

`User-agent: *`
`Disallow: /`

Block a specific HTML page: `<meta name="robots" content="noindex">` in `<head>`