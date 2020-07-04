FROM php:7.4-fpm

#SET php.ini
RUN mv $PHP_INI_DIR/php.ini-development $PHP_INI_DIR/php.ini \
	&& sed -i -e "s|;session.save_path = "/tmp"|session.save_path = "/tmp" |" /usr/local/etc/php/php.ini \
	&& sed -i -e "s|;extension=php_openssl.dll|extension=php_openssl.dll |" /usr/local/etc/php/php.ini

#INSTALL
## default utility
RUN apt-get update \
	&& apt-get install -y git

## zip
RUN apt-get -y install zip unzip libzip-dev \
	&& docker-php-ext-install zip

## imagick
RUN apt-get install -y --no-install-recommends \
		libmagickwand-dev \
	&& pecl install imagick-3.4.3 \
	&& docker-php-ext-enable imagick

## gd
RUN	apt-get install -y \
		gcc \
		make \
		libpng-dev \
		libmcrypt-dev \
		libfreetype6-dev \
        libjpeg62-turbo-dev \
    && docker-php-ext-configure gd --with-freetype --with-jpeg \
    && docker-php-ext-install -j$(nproc) gd

## xml
RUN apt install -y libxml2-dev \
	&& docker-php-ext-install soap xmlrpc

## redis
RUN pecl install redis \
	&& docker-php-ext-enable redis

## etc (xdebug bcmath pdo_mysql composer)
RUN pecl install xdebug-2.8.1 \
	&& docker-php-ext-enable xdebug \
	&& docker-php-ext-install bcmath pdo_mysql

COPY ./xdebug.ini $PHP_INI_DIR/conf.d/

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer