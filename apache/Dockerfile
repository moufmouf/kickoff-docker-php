FROM php:7-apache

# Adds configuration files
ADD php.ini /usr/local/etc/php/php.ini
ADD ext-apcu.ini /usr/local/etc/php/conf.d/ext-acpu.ini

# installs missing php dependencies
RUN pecl install apcu-5.1.5 \
    && pecl install xdebug-2.4.0

RUN apt-get update \
    && apt-get install -y zlib1g-dev # dependency for php zip extension

RUN docker-php-ext-install pdo_mysql \
    && docker-php-ext-install zip

# enables Apache's rewrite module
RUN a2enmod rewrite

# installs composer
RUN curl -sS https://getcomposer.org/installer \
    | php -- --install-dir=/usr/local/bin --filename=composer

# installs git
RUN apt-get update \
    && apt-get -y install git

# installs Node.js and npm
RUN curl -sL https://deb.nodesource.com/setup_4.x | bash - \
    && apt-get update \
    && apt-get install -y nodejs \
    && ln -s /usr/bin/nodejs /usr/local/bin/node \
    && ln -s /usr/bin/npm /usr/local/bin/npm

# defines entry point
COPY entrypoint.sh /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

# creates composer / npm user
RUN useradd --create-home --shell /bin/bash custom_user \
    && echo 'custom_user:custom_user' | chpasswd \
    && adduser custom_user sudo
