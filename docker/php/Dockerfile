FROM php:8.2-fpm

ARG UID=1000
ARG GID=1000

RUN apt-get update && apt-get install -y \
    git unzip libzip-dev libonig-dev libxml2-dev mariadb-client curl libicu-dev \
    && docker-php-ext-install pdo pdo_mysql zip mbstring exif pcntl bcmath intl

RUN groupadd -g ${GID} laravel \
    && useradd -u ${UID} -g laravel -m laravel \
    && usermod -aG www-data laravel

COPY --from=composer:2.6 /usr/bin/composer /usr/bin/composer

RUN cp /usr/local/etc/php/php.ini-development /usr/local/etc/php/php.ini

RUN pecl install xdebug \
    && docker-php-ext-enable xdebug

# Copiar antes de mudar de usuário
COPY docker/php/xdebug.ini /usr/local/etc/php/conf.d/xdebug.ini
COPY docker/php/custom.ini /usr/local/etc/php/conf.d/custom.ini

RUN curl -fsSL https://deb.nodesource.com/setup_22.x | bash - \
    && apt-get install -y nodejs

WORKDIR /var/www/html
USER laravel
EXPOSE 9000
