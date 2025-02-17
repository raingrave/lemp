FROM php:8.0.0-fpm-alpine

RUN apk add --no-cache openssl bash mysql-client
RUN docker-php-ext-install pdo pdo_mysql

RUN apk add --no-cache libpng libpng-dev && docker-php-ext-install gd && apk del libpng-dev

RUN apk add --no-cache $PHPIZE_DEPS \
	&& pecl install xdebug \
	&& docker-php-ext-enable xdebug

ENV DOCKERIZE_VERSION v0.6.1
RUN wget https://github.com/jwilder/dockerize/releases/download/$DOCKERIZE_VERSION/dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && tar -C /usr/local/bin -xzvf dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz \
    && rm dockerize-linux-amd64-$DOCKERIZE_VERSION.tar.gz

WORKDIR /var/www
RUN rm -rf /var/www/html

COPY ./php.ini.development "$PHP_INI_DIR/php.ini"

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN ln -s public html

EXPOSE 9000

ENTRYPOINT ["php-fpm"]
