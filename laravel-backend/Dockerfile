FROM composer:2.6 AS builder

WORKDIR /app

COPY database/ database/
COPY composer.json composer.lock ./
RUN composer install
COPY . .
RUN composer dump-autoload --optimize

FROM php:8.2-fpm AS final

RUN apt-get update && apt-get install -y \
    libzip-dev \
    zip \
    unzip \
    && docker-php-ext-install pdo pdo_mysql zip

WORKDIR /var/www/html
COPY --from=builder /app .
COPY .env.production .env

RUN chown -R www-data:www-data storage bootstrap/cache
RUN chmod -R 775 storage bootstrap/cache

CMD ["php-fpm"]
