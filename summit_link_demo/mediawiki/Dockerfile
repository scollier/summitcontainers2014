FROM scollier/apache
MAINTAINER Stephen Tweedie <sct@redhat.com>

# Basic RPM install...
RUN yum -y update; yum clean all

# Install:
#  Mediawiki, obviously
#  php, because mediawiki doesn't by itself install php into apache
#  php-mysqlnd: this image will be configured to run against the 
#               Fedora-Dockerfiles mariadb image so we need the mysqld
#               client support for php
RUN yum -y install mediawiki php php-mysqlnd; yum clean all

# Now wiki data.  We'll expose the wiki at $host/wiki, so the html root will be
# at /var/www/html/wiki; to allow this to be used as a data volume we keep the
# initialisation in a separate script.

ADD ./config.sh /config.sh
ADD ./run-apache.sh /run-apache.sh
ADD ./LocalSettings.php /var/www/html/wiki/
RUN chmod +x /run-apache.sh
RUN chmod -v +x /config.sh
RUN /config.sh

# localhost:/wiki/mw-config should now be available to configure mediawiki.

# Add script to update the IP address of a linked mariadb container if
# needed:

EXPOSE 80

ADD run-mw.sh /run-mw.sh
RUN chmod -v +x /run-mw.sh
CMD ["/run-mw.sh"]
