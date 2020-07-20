FROM php:7.1.20-apache

RUN apt-get -y update --fix-missing \
    &&  apt-get upgrade -y \
# Install useful tools
    && apt-get -y install apt-utils vim wget dialog net-tools \
# Install important libraries
    && apt-get -y install --fix-missing apt-utils build-essential git curl libcurl3 libcurl3-dev zip libldap2-dev \
    && apt-get -y install libmcrypt-dev libsqlite3-dev libsqlite3-0 mysql-client zlib1g-dev libicu-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev
# Composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# Install xdebug
RUN pecl install xdebug-2.5.0
RUN docker-php-ext-enable xdebug

# Other PHP7 Extensions

RUN docker-php-ext-install mcrypt \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_sqlite \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install curl \
    && docker-php-ext-install tokenizer \
    && docker-php-ext-install json \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install ldap \
    && mkdir /etc/ssl/certs/2020 \
    && docker-php-ext-install zip \
    && docker-php-ext-install -j$(nproc) intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd

# Enable apache modules
RUN a2enmod rewrite headers \
    && a2enmod rewrite \
    && a2ensite default-ssl \
    && a2enmod ssl

EXPOSE 80 443
ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
