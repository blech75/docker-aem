FROM ubuntu:14.04

# install apache packages
RUN	apt-get update && \
	apt-get install -y apache2 apache2-doc curl

# configure apache modules
# FIXME: vet these and remove as necessary
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

# clear out existing docroot; create new dir and allow write access to
# apache (dispatcher module)
#
# NOTE: the group passed to chgrp is defined in /etc/apache2/envvars
RUN rm -rf /var/www/* && \
	mkdir -p /var/www/dispatcher-publish && \
	chgrp -R www-data /var/www/dispatcher-publish && \
	chmod g+w /var/www/dispatcher-publish

COPY	ports.conf /etc/apache2/ports.conf

# FIXME: these confs should really live in sites-available and be symlinked to
# sites-enabled
COPY	80-dispatcher.conf /etc/apache2/sites-enabled/80-dispatcher.conf
COPY	81-author.conf /etc/apache2/sites-enabled/81-author.conf
COPY	82-publish.conf /etc/apache2/sites-enabled/82-publish.conf

COPY	dispatcher.so /usr/lib/apache2/modules/dispatcher.so
COPY	dispatcher.conf /etc/apache2/conf-enabled/dispatcher.conf
COPY	dispatcher.any /etc/apache2/conf.d/dispatcher.any

# set TERM so basic things like screen clearing and 'top' will work.
ENV TERM xterm

# see /etc/apache/envvars for config vars used by apache2ctl
#
# NOTE: using exec form to avoid being a child of 'sh -c' and to allow for
# receiving signals from 'docker stop'. see
# https://docs.docker.com/engine/reference/builder/#entrypoint
#
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
