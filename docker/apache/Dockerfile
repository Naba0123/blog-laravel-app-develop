FROM php:7.4.16-apache
COPY ./php.ini /usr/local/etc/php/
COPY ./*.conf /etc/apache2/sites-enabled/

# タイムゾーン設定
ENV TZ Asia/Tokyo
RUN echo "${TZ}" > /etc/timezone \
   && dpkg-reconfigure -f noninteractive tzdata

RUN apt-get update \
  && apt-get install -y libzip-dev libpq-dev mariadb-client unzip less nodejs npm\
  && docker-php-ext-install zip pdo_mysql mysqli \
  && docker-php-ext-enable mysqli

# デフォルトは rewrite が無効になっている
RUN cd /etc/apache2/mods-enabled \
    && ln -s ../mods-available/rewrite.load

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer
ENV COMPOSER_ALLOW_SUPERUSER 1
ENV COMPOSER_HOME /composer
ENV PATH $PATH:/composer/vendor/bin

WORKDIR /var/www/html

RUN composer global require "laravel/installer"