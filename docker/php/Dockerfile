FROM php:7.2-fpm-alpine

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}
ARG USERNAME=appuser
ARG TZ=Asia/Tokyo
ENV TZ ${TZ}

COPY docker/php/php.ini /usr/local/etc/php/php.ini
COPY . /var/www/html/

RUN addgroup -g ${PGID} ${USERNAME} \
    && adduser -D -u ${PUID} -G ${USERNAME} ${USERNAME} \
    && apk update && apk add tzdata \
    && cp /usr/share/zoneinfo/$TZ /etc/localtime \
    && apk update && apk add \
    zlib-dev \
    openssl \
    && docker-php-ext-install \
    pdo_mysql \
    zip \
    && rm -rf /var/cache/apk/* \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin --filename=composer \
    && mkdir -p /var/sessions \
    && chown -R www-data /var/sessions \
    && chown -R ${USERNAME}:${USERNAME} /var/www/html

USER ${USERNAME}
ENV PATH $PATH:/home/${USERNAME}/.composer/vendor/bin
WORKDIR /var/www/html
RUN composer config -g repos.packagist composer https://packagist.jp \
    && composer global require hirak/prestissimo \
    && composer install

USER root
