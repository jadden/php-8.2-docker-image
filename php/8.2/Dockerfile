FROM php:8.2-cli

RUN apt-get update && apt-get install -y \
    git \
    curl \
    unzip \
    libpng-dev \
    libjpeg-dev \
    libfreetype6-dev \
    libzip-dev \
    && curl -fsSL https://github.com/mlocati/docker-php-extension-installer/releases/latest/download/install-php-extensions \
    -o /usr/local/bin/install-php-extensions && chmod +x /usr/local/bin/install-php-extensions

RUN install-php-extensions curl ftp fileinfo pdo_mysql mysqli openssl pdo_sqlite gd mbstring zip bcmath intl exif

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer