FROM php:7.2-apache
LABEL maintainer="Gatovsky"

ARG USERID=1000
ARG GROUPID=1000
ARG USERNAME=appuser

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

RUN groupadd --gid ${GROUPID} ${USERNAME} \
        && useradd --uid ${USERID} --gid ${GROUPID} --shell /bin/bash -m ${USERNAME}
        # [Optional] Add sudo support. Omit if you don't need to install software after connecting.
        # && apt-get update \
        # && apt-get install -y sudo \
        # && echo $USERNAME ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/$USERNAME \
        # && chmod 0440 /etc/sudoers.d/$USERNAME

COPY  . /var/www/html/
RUN chown -R ${USERNAME}:${USERNAME} /var/www/html
WORKDIR /var/www/html/

USER ${USERNAME}

EXPOSE 80

CMD ["apache2-foreground"]
