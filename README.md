Python 2.7.4 + Nginx + uWSGI + Django on Openshift
==================================================


Setting up Openshift
--------------------

Create the Openshift application with a DIY and Postgresql 8.4 cartridge (also works on MySQL and SQLite out-of-the-box):

    $ rhc app create <exampleapp> diy-0.1 postgresql-8.4
    $ cd <exampleapp>
    $ git remote add upstream -m master git://github.com/seedofjoy/openshift-diy-nginx-uwsgi-django.git
    $ git pull -s recursive -X theirs upstream master
    $ git push


Configuration
-------------
* uWSGI and Nginx configs are located in configs/ dir. Environments variables in this configs will substitute on deploy stage.
* If you want change the name of the Django project, please change it in the file .appname too.
* Django project is configured to work with PostgreSQL, MySQL or SQLite (in this priority order).
* Also this Django project will work on local machine: it will use SQLite, static and media folders will located in project folder.


Pre build stage
---------------

The 'pre_build' hook script performs the following actions:
* Installs Python 2.7.4
* Installs Virtualenv 1.9.1 and creates virtualenv in $OPENSHIFT_DATA_DIR/virtualenv
* Installs Nginx 1.4.0

You can install any version as you want, just change versions variables in this script


Build stage
-----------

* Installs a pip packages from 'requirements.txt' (Django>=1.5, psycopg2, uwsgi, Pillow)

Feel free to edit this file, but leave uWSGI and Django.


Deploy stage
------------

* Creates static and media dirs ($OPENSHIFT_DATA_DIR/static/static and $OPENSHIFT_DATA_DIR/static/media) for Django
* Substitute environments variables to Nginx and uWSGI configs and copy its to $OPENSHIFT_DATA_DIR


