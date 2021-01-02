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
## Flask


---
## Django
1. Setup
    - create virtualenv, pip install django, output django version to requirements.txt
    - create project: `startproject <project_name>`
    - create first app: `<project_name>/manage.py startapp <app_name>`
2. Setup routing for first app
    - add `include` function to <project_name>/urls.py: `from django.urls import path, include`
    - create `urls.py` in first app directory
    - add path to first app: `path('', include('<app_name>.urls'))`
    - add `path` function and `urlpatterns` list to first app's `urls.py`
    - add `views.py` to first app's `urls.py`: `from . import views`
    - add a path to first app's `urlpatterns`: `path('', views.home, name='<app_name>-home')`
3. Create views
    - create view functions: `def home(request):`
    - add context: `context = {...}`
    - render template: `return render(request, '<app_name>/home.html', context)` 
4. Add templates
    - create `templates` directory within first app's directory
    - create a app directory within the templates directory: `<project_name>/<app_name>/templates/<app_name>`
    - create HTML templates within the `templates/<app_name>` directory
    - add template inheritence: create a `base.html` template to be extended in all other templates
        - (Optional) Add bootstrap in `base.html` from bootstrap starter template (metadata, css, and javascript)
    - add HTML snippets
    - replace anchor tag hrefs with url names: `{% <app-name>-home %}`
5. Add static assests and media directories
    - add static directory: `<app_name>/static/<app_name>`
    - add main css file: `<app_name>/static/<app_name>/main.css`
    - load static directory within template: `{% load static %}`
    - load a static asset: `<link rel="stylesheet" href="{% static '<app-name>/main.css' %">`
    - in `settings.py` add the following:
        - full path to static directory: `STATIC_ROOT = os.path.join(BASE_DIR, 'static')`
        - public URL of the static directory and how it will be accessed through the browser: `STATIC_URL = '/static/'`
        - full path to media directory: `MEDIA_ROOT = os.path.join(BASE_DIR, 'media')`
        - public URL of the media directory and how it will be accessed through the browser: `MEDIA_URL = '/media'`
6. Create admin user
    - `<project_name>/manage.py migrate`
    - `<project_name>/manage.py createsuperuser`
7. Create models
    - inherit from the Model class: `class <model_name>(models.Model)`
    - reference a user foreign key: `from django.contrib.auth.models import User`
        - delete anything referencing the user: `user = models.ForeignKey(User, on_delete=models.CASCADE)`
        - retain anything referencing the user and set the reference to null: `user = models.ForeignKey(User, on_delete=models.SET_NULL, null=True)`
    - util for using mutable time fields: `from django.utils import timezone`
        - mutable date: `date_created = models.DateTimeField(default=timezone.now)`
        - use data at time of add/edit: `date_updated = models.DateTimeField(auto_now=True)`
    - add descriptive text when querying: `def __str__(self): return self.name`
    - run `makemigrations`
    - view raw SQL: `manage.py sqlmigrate <app_name> <migration_number>`
    - apply migrations: `migrate`
    - register in admin site: `admin.site.register(<model_name>)`
        - import `from .models import <model_name>`
8. Interact with Models in Django Python shell
    - `manage.py shell`
    - import users: `from django.contrib.auth.models import User`
    - import models: `from <app_name>.models import <model_name>`
    - get all, get id, first, or last objects: `User.objects.all()` or `.get(1)`or `.first()` or `.last()`
    - filter results: `user = User.objects.filter(username='<string>).first()`
    - check attributes: `user.pk`
    - get all objects referencing a user: `user.<model_name>_set.all()`
        - add new object for this user: `user.<model_name>_set.create(<model_attributes>)`
9. Query database in view
    - import model in views file: `from .models import <model_name>`
    - add it into the response context: `context = {'announcements': Announcement.objects.all()`
10. User creation
    - default form: `from django.contrib.auth.forms import UserCreationForm`
    - get form data: `f request.method == 'POST': form = UserRegisterForm(request.POST)`
    - validate form: `if form.is_valid():`
    - create user: `form.save()`
    - format form with `django-crispy-forms`
        - add to installed apps: `'crispy_forms',`
        - set frontend framework in django settings: `CRISPY_TEMPLATE_PACK = 'bootstrap4'`
    - log user in after registration
        - required imports: `from django.contrib.auth import authenticate, login`
        - `if form.is_valid():` then `login(request, authenticate(username=username, password=password))`
    - update redirect to specific page after login in django settings: `LOGIN_REDIRECT_URL = 'profile'`
11. Restrict pages to logged in users
    - import decorator: `from django.contrib.auth.decorators import login_required`
    - add decorator to any view function returning a restricted page: `@login_required`
    - for class-based views:
        - import mixin: `from django.contrib.auth.mixins import LoginRequiredMixin`
        - include within class: `class AccountView(LoginRequiredMixin, View):`
    - alternatively, include for specific class method:
        - import: `from django.utils.decorators import method_decorator`
        - method decorator: `@method_decorator(login_required)`
12. Extend Users model
    - import User model: `from django.contrib.auth.models import User`
    - create extended model: `class Profile(models.Model):`
    - create a one-to-one relationship with a user model: `user = models.OneToOneField(User, on_delete=models.CASCADE)`
    - add to admin site in admin.py: `from .models import Profile` `admin.site.register(Profile)`
13. Django Signals (to automatically execute some behavior)
    - import post-save signal: `from django.db.models.signals import post_save`
    - import sender: `from django.contrib.auth.models import User`
    - import receiver: `from django.dispatch import receiver`
    - import related models: `from .models import Profile`

### Jinja
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
## VSCode
- pull up intellisense on mac: `Option + Esc` (`Alt` on Windows)

#### User Snippets
Django code block snippets for html templates:  
```
{
	"Django for-loop code block": {
		"prefix": ["for", "django", "code", "code-block"],
		"body": ["{% for ${1:element} in ${2:iterable} %}", "\t$0","{% endfor %}"]
	},
	"Django if code block": {
		"prefix": ["if", "django", "code", "code-block"],
		"body": ["{% if ${1:condition} %}", "\t$0","{% endif %}"]
	},
	"Django if/else code block": {
		"prefix": ["if/else", "django", "code", "code-block"],
		"body": ["{% if ${1:condition} %}", "\t$0", "{% else %}", "\t", "{% endif %}"]
	},
	"Django if/elif code block": {
		"prefix": ["if/elif", "django", "code", "code-block"],
		"body": ["{% if ${1:condition} %}", "\t$0", "{% elif ${2:condition} %}", "\t", "{% endif %}"]
	},
	"Django if/elif/else code block": {
		"prefix": ["if/elif/else", "django", "code", "code-block"],
		"body": ["{% if ${1:condition} %}", "\t$0", "{% elif ${2:condition} %}", "\t", "{% else %}", "\t", "{% endif %}"]
	}
}
```

#### Set Syntax
Command Pallete > languages  
"Change Language Mode"  
e.g. "SQL Formatter" Extension

#### Format File in Syntax
Shift + Option + F (Mac)




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