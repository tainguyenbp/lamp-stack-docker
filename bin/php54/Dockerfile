FROM php:5.4-apache

# Backup and clean source.list file
RUN cp /etc/apt/sources.list /etc/apt/sources.list.old && \
    cat /dev/null > /etc/apt/sources.list

# Fix the source.list for jessie
RUN printf "deb http://archive.debian.org/debian/ jessie main\n" > /etc/apt/sources.list && \
    printf "deb-src http://archive.debian.org/debian/ jessie main\n" >>  /etc/apt/sources.list && \
    printf "deb http://security.debian.org jessie/updates main\n" >>  /etc/apt/sources.list && \
    printf "deb-src http://security.debian.org jessie/updates main" >>  /etc/apt/sources.list

RUN apt-get -y update --force-yes && apt-get upgrade -y --force-yes \

# Install tools && libraries
RUN apt-get -y install --fix-missing apt-utils vim wget dialog \
    build-essential git curl libcurl3 libcurl3-dev zip \
    libmcrypt-dev libsqlite3-dev libsqlite3-0 mysql-client \
    zlib1g-dev libicu-dev libldb-dev libldap2-dev libfreetype6-dev libjpeg62-turbo-dev libpng-dev net-tools \
    libapache2-mod-rpaf libpng12-dev --force-yes \
    && rm -rf /var/lib/apt/lists/* /root/.cache/tmp/* /var/tmp/* \

# Composer
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# PHP5 Extensions
RUN docker-php-ext-install curl \
    && docker-php-ext-install tokenizer \
    && docker-php-ext-install json \
    && docker-php-ext-install mcrypt \
    && docker-php-ext-install pdo_mysql \
    && docker-php-ext-install pdo_sqlite \
    && docker-php-ext-install mysql \
    && docker-php-ext-install mysqli \
    && docker-php-ext-install zip \
    && docker-php-ext-install intl \
    && docker-php-ext-install mbstring \
    && docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ \
    && docker-php-ext-install ldap \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install gd \
    && pecl install xdebug-2.4.0RC4 && docker-php-ext-enable xdebug \
    && echo "xdebug.remote_enable=1" >> /usr/local/etc/php/php.ini \
    && mkdir /etc/ssl/certs/2020 \
# Enable apache modules
    && a2enmod rewrite headers \
    && a2enmod rewrite rpaf \
    && a2enmod rewrite \
    && a2ensite default-ssl \
    && a2enmod ssl

EXPOSE 80 443

ENTRYPOINT ["/usr/sbin/apache2ctl", "-D", "FOREGROUND"]
