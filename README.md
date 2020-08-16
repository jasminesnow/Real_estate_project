Based on ['Python Django Dev To Deployment course'](https://learning.oreilly.com/videos/python-django-dev/9781838641283)



_________

WORK FLOW:

Project:
1. **Create project** - django-admin startproject real_estate
2. **Add gitignore & readme**

Pages app:
1. **Create app _pages_** - python manage.py startapp pages
2. **Add _pages_ to real_estate/settings.py INSTALLED_APPS list**
3. **Create pages/urls.py**
4. **Create views in pages/views.py to return index.html & about.html**
5. **Include pages/urls.py to real_estate/urls.py**

Static files (css, img, js, webfonts):
1. **Create real_estate/static with /css, /img, /js, /webfonts**
2. **Update real_estate/settings.py to add STATIC_ROOT, STATICFILES_DIRS**
3. **Collect static files from multiple apps into a single path for frontend web server (e.g. nginx)** - python manage.py collectstatic

Templates (website htmls):
1. **Create /templates**
2. **Update real_estate/settings.py TEMPLATES->DIRS to lead to /templates**
3. **Create base.html, pages/index.html & pages/about.html** - base.html contains nav bar, header & footer references, 
index and about - extend base and have only content
4. **Create /partials with _navbar.html, _topbar.html, _footer.html**
5. **Point to static img inside of htmls with {% static 'name' %} and to correct url page with {% url 'name' %}**

Listings & Realtors apps:
1. **Create app _listings_** - python manage.py startapp listings
2. **Create app _realtors_** - python manage.py startapp realtors
3. **Add _listings_ and _realtors_ to real_estate/settings.py INSTALLED_APPS list**
4. **Create templates/listings with listings.html, listing.html, search.html** - not for realtors, 
they wouldn't have separate realtors page (will be only as part of existing pages)
5. **Create listings/urls.py** - not for realtors, same reason as above
6. **Include listings/urls.py to real_estate/urls.py**
7. **Create views in listings/views.py to return listings.html, listing.html, search.tml**

Connect Postgres:
1. **Create new db** - using PGAdmin if exists local server, else install [server+PGAdmin](https://www.postgresql.org/download/) 
2. **Install psycopg2 (Postgres adapter)** - pip install psycopg2, pip install psycopg2-binary
3. **Update real_estate/settings.py DATABASES section**
4. **Run init migration** - python manage.py migrate
5. **Create superuser** - python manage.py createsuperuser

Create models:
1. **Create listings/models.py with Listing model**
2. **Create realtors/models.py with Realtor model**
3. **Create migrations** - python manage.py makemigrations
4. **Run migrations** - python manage.py migrate

Admin (+media):
1. **Register Listing in listings/admin.py**
2. **Register Realtor in realtors/admin.py**
3. **Add MEDIA_ROOT & URL to real_estate/settings.py** - all uploads via Admin will be stored in autogenerated /media
4. **Add media to real_estate/urls.py with '+ static...'** - to display media files on UI later
5. **Create realtors and listings entries in Admin**
6. **Create css/admin.py to style admin in brand colors**
7. **Customize admin listings view by adding class in listings/admin.py with admin fields to display, filter etc.**
8. **Customize admin realtors view by adding class in realtors/admin.py with admin fields to display, filter etc.**

Pull DB data into listings html:
1. **Request data in listings/views.py with Django ORM and pass context to listings.html**
2. **For listings.html loop through listings parameter and return listing.title, listing.price etc.** - html return 
dynamic data instead of hardcoded values
3. **Add humanize to real_estate/settings.py**
4. **Load humanize into listings.html and use it for formatting (20000 to 20`000 etc.)**

Add pagination:
1. **Add paginator to listings/views.py**
2. **Update listings.html to display previous/current/available/next pages**

Pull DB data into rest of the htmls (home, about, listing, search):
1. **Request data in listings/views.py with Django ORM and pass context to htmls**
2. **Loop through listings and realtors for templates**
3. **Move listings section into /partials and update index, listings and search.html** - to avoid html repetition in templates

Search:
1. **Create choices.py in /listings with list/dictionary options**
2. **Pass choices to pages/views.py (index) and listings/views.py (search)**
3. **Update index.html and search.html to work with choices**
4. **Move search form section into /partials and update index and search.html** - to avoid html repetition in templates
5. **Update listings/views.py to filter queryset according to search params**
6. **Update partials/_search_form.html to keep search params on UI after the run**

Accounts app:
1. **Create accounts app** - python manage.py startapp accounts
2. **Add _accounts_ to real_estate/settings.py INSTALLED_APPS list**
3. **Create accounts/urls.py**
4. **Create views in accounts/views.py linked to urls**
5. **Include accounts/urls.py to real_estate/urls.py**
6. **Create templates/accounts with register.html, login.html, dashboard.html**

Message alerts:
1. **Add MESSAGE_TAGS to real_estate/settings.py**
2. **Create partials/_alerts.html**
3. **Include _alerts.html to login.html & register.html**

Registration, Login & Logout:
1. **Update _registration_ method in accounts/views.py to validate & save user**
2. **Update _login_ method in accounts/views.py to login user**
3. **Update _logout_ method in accounts/views.py to logout user**
4. **Update relative templates**

Contacts app:
1. **Create app _contacts_** - python manage.py startapp contacts
2. **Add _contacts_ to real_estate/settings.py INSTALLED_APPS list**
3. **Create contacts/urls.py**
4. **Add contacts to real_estate/urls.py**
5. **Create contacts/models.py with Contact model**
6. **Create migrations** - python manage.py makemigrations contacts
7. **Run migrations** - python manage.py migrate contacts
8. **Add contacts to contacts/admin.py**
9. **Update listing.html to pre-populate fields for logged in users, include alerts**
9. **Create contact view in contacts.views.py to save form data** - with additional logic (send email to realtor, detect if form was already submitted)
10. **Update real_estate/settings.py with email config**
11. **Update dashboard in accounts/views.py and accounts/dashboard/html to work with db data** 

Create requirements.txt:
1. **Create requirements.txt** - pip freeze > requirements.txt (to see libs only - pip freeze)
______________

DEPLOYMENT WITH DIGITAL OCEAN 
([summary for Digital Ocean](https://gist.github.com/bradtraversy/cfa565b879ff1458dba08f423cb01d71)):

Create droplet with SSH key:
1. **Create ssh key** - ssh-keygen -> enter path like /c/Users/.../.ssh/id_rsa_digitalocean (Git Bush on Wind10)
2. **Get public ssh key** - cat ./.ssh/id_rsa_digitalocean.pub -> copy
3. **Start creating Ubuntu droplet on [Digital Ocean](https://www.digitalocean.com/)**
4. **Copy ssh key into the droplet and hit Create**

Create new user (security provisions):
1. **Connect to server console directly via Digital Ocean** or 
**connect via local machine using Git Bush -> ssh root@111.11.111.11** - if refused -> ssh-add ./.ssh/id_rsa_digitalocean -> || if could not open connection -> eval ssh-agent -s ("ssh-agent -s" in ``) and repeat ssh-add command
2. **Create new user** - adduser djangoadmin_v
3. **Give new user admin privileges** - usermod -aG sudo djangoadmin_v
4. **Open user folder and create .ssh folder** - cd /home/djangoadmin_v -> mkdir .ssh -> cd .ssh
5. **Create authorized keys with ssh key** - nano authorized_keys (paste key -> Ctrl+X -> Y to save -> Enter to exit) -> cat authorized_keys (to view key)
6. **Exit server** - exit
7. **Disable root login** - nano /etc/ssh/sshd_config ->PermitRootLogin no ; PasswordAuthetincation no
8. **Reaload sshd service** - sudo systemctl reload sshd

Setup Firewall:
1. **Allow OpenSSH** - sudo ufw app list -> sudo ufw allow OpenSSH
2. **Enable firewall** - sudo ufw enable -> sudo ufw status

Install software:
1. **Update packages** - sudo apt update -> sudo apt upgrade
2. **Install python packages** - sudo apt install python3-pip python3-dev libpq-dev postgresql postgresql-contrib nginx curl

Create POSTGRES DB and Admin:
1. **Create postgres DB & ADMIN** - sudo -u postgres psql -> CREATE DATABASE website_prod; -> CREATE USER dbadmin WITH PASSWORD '123!';
2. **Setup admin role** - ALTER ROLE dbadmin SET client_encoding TO 'utf8'; -> ALTER ROLE dbadmin SET default_transaction_isolation TO 'read committed'; -> ALTER ROLE dbadmin SET timezone TO 'UTC';
3. **Grant all privileges to admin** - GRANT ALL PRIVILEGES ON DATABASE re_prod TO dbadmin; -> \q

Create Virtual Environment:
1. **Install the python3-venv package** - sudo apt install python3-venv
2. **Create project directory** - mkdir pyapps -> cd pyapps
3. **Create venv & activate** - python3 -m venv ./venv -> source venv/bin/activate
4.

