# DOCKER-VERSION 0.4.0

# from	ubuntu:12.04
FROM shykes/nodejs:latest
run	echo 'deb http://us.archive.ubuntu.com/ubuntu/ precise universe' >> /etc/apt/sources.list
run	apt-get -y update

# Install required packages
run	apt-get -y install python-ldap python-cairo python-django python-twisted python-django-tagging python-simplejson python-memcache python-pysqlite2 python-support python-pip gunicorn supervisor nginx-light
run	pip install whisper
run	pip install --install-option="--prefix=/var/lib/graphite" --install-option="--install-lib=/var/lib/graphite/lib" carbon
run	pip install --install-option="--prefix=/var/lib/graphite" --install-option="--install-lib=/var/lib/graphite/webapp" graphite-web

RUN mkdir /data /var/run/sshd

RUN apt-get install -q -y wget libssl-dev

RUN wget https://github.com/etsy/statsd/archive/v0.6.0.tar.gz --no-check-certificate
RUN tar -xvzf v0.6.0.tar.gz

# Add system service config
add	./nginx.conf /etc/nginx/nginx.conf
add	./supervisord.conf /etc/supervisor/conf.d/supervisord.conf

# Add graphite config
add	./initial_data.json /var/lib/graphite/webapp/graphite/initial_data.json
add	./local_settings.py /var/lib/graphite/webapp/graphite/local_settings.py
add	./carbon.conf /var/lib/graphite/conf/carbon.conf
add	./storage-schemas.conf /var/lib/graphite/conf/storage-schemas.conf
run	mkdir -p /var/lib/graphite/storage/whisper
run	touch /var/lib/graphite/storage/graphite.db /var/lib/graphite/storage/index
run	chown -R www-data /var/lib/graphite/storage
run	chmod 0775 /var/lib/graphite/storage /var/lib/graphite/storage/whisper
run	chmod 0664 /var/lib/graphite/storage/graphite.db
run	cd /var/lib/graphite/webapp/graphite && python manage.py syncdb --noinput

ADD config.js /data/config.js
run	chown -R www-data /data
run	chown -R www-data /data/config.js

RUN cat /data/config.js

# Nginx
expose	:80
# Carbon line receiver port
expose	:2003
# Carbon pickle receiver port
expose	:2004
# Carbon cache query port
expose	:7002

# statsd
EXPOSE 8125:8125/udp
EXPOSE 8126:8126

cmd	["/usr/bin/supervisord"]

# vim:ts=8:noet:
