FROM php:8.4-fpm-alpine

RUN apk add --no-cache \
    git \
    unzip \
    libpq-dev \
    icu-dev \
    oniguruma-dev \
    linux-headers \
    $PHPIZE_DEPS \
    && docker-php-ext-install \
        pdo \
        pdo_pgsql \
        bcmath \
        intl \
    && apk del $PHPIZE_DEPS

COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

WORKDIR /var/www/html

RUN addgroup -g 1000 appuser && adduser -G appuser -u 1000 -D appuser

USER appuser