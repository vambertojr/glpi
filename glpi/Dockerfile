FROM debian:10.4

ARG VERSION=9.5.1

ENV DEBIAN_FRONTEND noninteractive

RUN apt update \
&& apt install --yes --no-install-recommends \
apache2 \
php7.3 \
php7.3-mysql \
php7.3-ldap \
php7.3-xmlrpc \
php7.3-fileinfo \
php7.3-imap \
php7.3-json \
curl \
php7.3-curl \
php7.3-gd \
php7.3-mbstring \
php7.3-xml \
php7.3-apcu-bc \
php-cas \
php7.3-intl \
php7.3-zip \
php7.3-bz2 \
cron \
wget \
ca-certificates \
jq \
&& rm -rf /var/lib/apt/lists/*

WORKDIR /var/www/html

ADD https://github.com/glpi-project/glpi/releases/download/${VERSION}/glpi-${VERSION}.tgz /var/www/html

RUN tar -zxvf /var/www/html/glpi-${VERSION}.tgz \
&& rm -rf glpi-${VERSION}.tgz \
&& chown www-data:www-data /var/www/html/glpi -R

COPY "./000-default.conf" /etc/apache2/sites-available/000-default.conf

RUN echo "*/2 * * * * www-data /usr/bin/php /var/www/html/glpi/front/cron.php &>/dev/null" >> /etc/cron.d/glpi \
&& service cron start

RUN sed -E -i -e 's/max_execution_time = 30/max_execution_time = 600/' /etc/php/7.3/cli/php.ini

RUN a2enmod rewrite && service apache2 restart && service apache2 stop

EXPOSE 80 443

ENTRYPOINT [ "apache2ctl", "-D", "FOREGROUND" ]
