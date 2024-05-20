FROM php:7.2-apache
LABEL maintainer="Gatovsky"

RUN a2enmod rewrite

RUN apt-get update && apt-get install -y \
        zlib1g-dev \
        libicu-dev \
        libxml2-dev \
        libpq-dev \
        vim \
        && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
        && docker-php-ext-install pdo pdo_mysql pdo_pgsql zip intl xmlrpc soap opcache \
        && docker-php-ext-configure pdo_mysql --with-pdo-mysql=mysqlnd \
        && apt-get clean \
        && rm -rf /var/lib/apt/lists/*

RUN groupadd -g 1000 www \
        && useradd -u 1000 -ms /bin/bash www

COPY  . /var/www/html/
RUN chown -R www:www /var/www/html
WORKDIR /var/www/html/

USER www

EXPOSE 80

CMD ["apache2-foreground"]
