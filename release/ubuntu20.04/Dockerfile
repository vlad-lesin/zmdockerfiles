FROM ubuntu:20.04
MAINTAINER Greg Cockburn

# Update base packages
RUN apt update \
    && apt upgrade --assume-yes

# Install pre-reqs
RUN apt install --assume-yes --no-install-recommends gnupg

# Configure Zoneminder PPA
RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys ABE4C7F993453843F0AEB8154D0BF748776FFB04 \
    && echo deb http://ppa.launchpad.net/iconnor/zoneminder-1.36/ubuntu focal main > /etc/apt/sources.list.d/zoneminder.list \
    && apt update

# Install zoneminder
RUN DEBIAN_FRONTEND=noninteractive apt install --assume-yes zoneminder \
    && a2enconf zoneminder \
    && a2enmod rewrite cgi

# Setup Volumes
VOLUME /var/cache/zoneminder/events /var/cache/zoneminder/images /var/lib/mysql /var/log/zm

RUN rm -rf /var/log/mysql
RUN ln -s /var/log/zm /var/log/mysql
RUN sed -i -e "s/Listen 80$/Listen 8080/g" /etc/apache2/ports.conf
RUN sed -i -e "s/<VirtualHost \*:80>/<VirtualHost \*:8080>/g" /etc/apache2/sites-enabled/000-default.conf

# Expose http port
EXPOSE 8080

# Configure entrypoint
COPY utils/entrypoint.sh /usr/local/bin/
RUN chmod 755 /usr/local/bin/entrypoint.sh
ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]
