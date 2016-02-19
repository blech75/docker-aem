FROM ubuntu:14.04

# install apache packages
RUN	apt-get update && \
	apt-get install -y apache2 apache2-doc

# configure apache modules
RUN a2enmod \
	headers \
	alias \
	ssl \
	rewrite \
	proxy \
	proxy_html \
	proxy_http \
	xml2enc

# make placholder ssl dir; remove default site since we don't need it
RUN mkdir -p /etc/apache2/ssl && \
	rm /etc/apache2/sites-enabled/000-default.conf

# clear out docroot and allow write access to apache (dispatcher module)
RUN rm /var/www/html/* && \
	chgrp -R www-data /var/www/html && \
	chmod g+w /var/www/html

COPY	ports.conf /etc/apache2/ports.conf

# FIXME: these confs should really live in sites-available and be symlinked to
# sites-enabled
COPY	80-dispatcher.conf /etc/apache2/sites-enabled/80-dispatcher.conf
COPY	81-author.conf /etc/apache2/sites-enabled/81-author.conf
COPY	82-publish.conf /etc/apache2/sites-enabled/82-publish.conf

COPY	dispatcher.so /usr/lib/apache2/modules/dispatcher.so
COPY	dispatcher.conf /etc/apache2/conf-enabled/dispatcher.conf
COPY	dispatcher.any /etc/apache2/conf.d/dispatcher.any

ENV APACHE_DOCUMENTROOT /var/www
ENV APACHE_RUN_USER www-data
ENV APACHE_RUN_GROUP www-data
ENV APACHE_LOG_DIR /var/web/log/apache2
ENV APACHE_PID_FILE /var/run/apache2.pid
ENV APACHE_RUN_DIR /var/run/apache2
ENV APACHE_LOCK_DIR /var/lock/apache2

CMD /usr/sbin/apache2ctl -D FOREGROUND
