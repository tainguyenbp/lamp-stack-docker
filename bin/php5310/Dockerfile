FROM ubuntu:12.04
MAINTAINER tainn@younetgroup.com

#RUN echo "Asia/Ho_Chi_Minh" > /etc/timezone
ENV TZ=Asia/Ho_Chi_Minh
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

VOLUME ["/var/www"]
RUN mkdir /var/run/apache2
RUN apt-get update && \
    apt-get install -y \
      apache2 \
      apache2-doc \
      apache2-utils \
      vim \
      net-tools \
      php5 \
      php5-cli \
      libapache2-mod-php5 \
      php5-gd \
      php5-ldap \
      php5-mysql \
      php5-pgsql \
      && rm -rf /var/lib/apt/lists/*

COPY ports.conf /etc/apache2/ports.conf

COPY run /usr/local/bin/run
RUN chmod +x /usr/local/bin/run
RUN a2enmod rewrite \
    && a2ensite default-ssl \
    && a2enmod ssl

EXPOSE 80 443
CMD ["/usr/local/bin/run"]
