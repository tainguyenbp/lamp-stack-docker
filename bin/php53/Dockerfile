FROM ubuntu:12.04

LABEL maintainer="support@computestacks.com"

RUN set -ex; \
    \
    apt-get update; \
    apt-get install -y \
            zip \
            unzip \
            libpng-dev \
            mysql-client \
            libxml2-dev \
            libbz2-dev \
            libmcrypt-dev \
            libmemcached-dev \
            libfreetype6-dev \
            libcurl4-gnutls-dev \
            libc-client-dev \
            libkrb5-dev \
            libxslt-dev \
            vim \
            zlib1g-dev \
            libicu-dev \
            g++ \
            wget \
            imagemagick \
            libmagickwand-dev \
            mcrypt \
            apache2 \
            php5 \
            php5-cli \
            libapache2-mod-php5 \
            php5-gd \
            php5-ldap \
            php5-mysql \
            php5-pgsql \
            php5-mcrypt \
            php5-curl \
            php5-json \
            php5-intl \
            net-tools \
            lsof \
    ; \
    rm -rf /var/lib/apt/lists/*;

RUN a2enmod rewrite \
#RUN a2enmod rewrite expires deflate \
    && mkdir /etc/ssl/certs/2020 \
#    && a2enmod headers \
#    && a2enmod php5 
#    && a2ensite default \
    && a2ensite default-ssl \
    && a2enmod ssl

COPY 000-default.conf /etc/apache2/sites-enabled/000-default
COPY ports.conf /etc/apache2/ports.conf
COPY httpd.conf /etc/apache2/httpd.conf
COPY sample /usr/src/sample
#COPY apache2.conf /etc/apache2/apache2.conf
COPY logging.conf /etc/apache2/conf.d/cslog.conf
COPY docker-entrypoint.sh /entrypoint.sh
COPY run /usr/local/bin/run

COPY vendor/cs.ini /etc/php5/apache2/conf.d/cs.ini

RUN usermod -u 1001 www-data \
    && groupmod -g 1001 www-data \
    && chmod +x /usr/local/bin/run \
    && chmod +x /entrypoint.sh

WORKDIR /var/www/html

ENTRYPOINT ["/entrypoint.sh"]
EXPOSE 80 443
CMD ["/usr/local/bin/run"]
