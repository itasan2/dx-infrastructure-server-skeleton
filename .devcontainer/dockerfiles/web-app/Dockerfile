ARG COMPOSER_VERSION="latest"
FROM composer:${COMPOSER_VERSION} AS my-composer
RUN echo "COMPOSER_VERSION:${COMPOSER_VERSION}"



FROM alpine:3.14

ARG TZ="UTC"
RUN set -x && \
    echo "TZ:${TZ}" && \
    apk add --no-cache \
        tzdata && \
    cp /usr/share/zoneinfo/${TZ} /etc/localtime

ARG PHP_MAJOR_VERSION="7"
RUN echo "PHP_MAJOR_VERSION:${PHP_MAJOR_VERSION}" && \
    apk add --no-cache \
        apache2 \
        curl \
        php${PHP_MAJOR_VERSION} \
        php${PHP_MAJOR_VERSION}-apache2 && \
    if [ "${PHP_MAJOR_VERSION}" = "8" ]; then \
        ln -s /usr/bin/php${PHP_MAJOR_VERSION} /usr/bin/php; \
    fi

# PHP extensions for Composer
RUN apk add --no-cache \
        php${PHP_MAJOR_VERSION}-phar \
        php${PHP_MAJOR_VERSION}-curl \
        php${PHP_MAJOR_VERSION}-zip

# install PHP Composer
COPY --from=my-composer /usr/bin/composer /usr/bin/composer
ARG COMPOSER_HOME="/.composer"
ENV COMPOSER_HOME=${COMPOSER_HOME} \
    COMPOSER_ALLOW_SUPERUSER=1 \
    PATH="$PATH:${COMPOSER_HOME}/vendor/bin"

# PHP extensions for PHPUnit
RUN apk add --no-cache \
        php${PHP_MAJOR_VERSION}-dom \
        php${PHP_MAJOR_VERSION}-xmlwriter

# PHP extensions for Laravel
RUN apk add --no-cache \
        php${PHP_MAJOR_VERSION}-bcmath \
        php${PHP_MAJOR_VERSION}-ctype \
        php${PHP_MAJOR_VERSION}-fileinfo \
        php${PHP_MAJOR_VERSION}-json \
        php${PHP_MAJOR_VERSION}-mbstring \
        php${PHP_MAJOR_VERSION}-openssl \
        php${PHP_MAJOR_VERSION}-pdo \
        php${PHP_MAJOR_VERSION}-tokenizer \
        php${PHP_MAJOR_VERSION}-session \
        php${PHP_MAJOR_VERSION}-xml \
        php${PHP_MAJOR_VERSION}-pdo_mysql && \
    composer global require laravel/installer

EXPOSE 80 8000
ENTRYPOINT ["/usr/sbin/httpd", "-D", "FOREGROUND"]
