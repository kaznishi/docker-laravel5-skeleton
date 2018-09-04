FROM php:7.2-fpm-alpine

ARG PUID=1000
ARG PGID=1000
ARG USERNAME=appuser
ARG TZ=Asia/Tokyo

COPY docker/php/php.ini /usr/local/etc/php/php.ini
COPY docker/php/www.conf.template /usr/local/etc/php-fpm.d/www.conf.template
COPY . /var/www/html/

RUN addgroup -g ${PGID} ${USERNAME} \
    && adduser -D -u ${PUID} -G ${USERNAME} ${USERNAME} \
    && apk update && apk add tzdata \
    && cp /usr/share/zoneinfo/$TZ /etc/localtime \
    && apk update && apk add \
    zlib-dev \
    openssl \
    gettext \
    && docker-php-ext-install \
    pdo_mysql \
    zip \
    && rm -rf /var/cache/apk/* \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && mkdir -p /var/sessions \
    && chown -R ${USERNAME}:${USERNAME} /var/sessions \
    && chown -R ${USERNAME}:${USERNAME} /var/www/html \
    && envsubst < /usr/local/etc/php-fpm.d/www.conf.template > /usr/local/etc/php-fpm.d/www.conf

USER ${USERNAME}
ENV PATH $PATH:/home/${USERNAME}/.composer/vendor/bin
WORKDIR /var/www/html
RUN composer config -g repos.packagist composer https://packagist.jp \
    && composer global require hirak/prestissimo \
    && composer install

USER root
