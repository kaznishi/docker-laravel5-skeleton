FROM php:7.2-fpm

ARG PUID=1000
ENV PUID ${PUID}
ARG PGID=1000
ENV PGID ${PGID}
ARG USERNAME=appuser
ARG TZ=Asia/Tokyo
ENV TZ ${TZ}

COPY docker/php/php.ini /usr/local/etc/php/php.ini
COPY . /var/www/html/

RUN groupadd -g ${PGID} ${USERNAME} && \
    useradd -u ${PUID} -g ${USERNAME} -m ${USERNAME} && \
    usermod -p "*" ${USERNAME} \
    && ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone \
    && apt-get update && apt-get install -y \
    build-essential \
    mysql-client \
    openssl \
    locales \
    zip \
    zlib1g-dev \
    && apt-get clean && rm -rf /var/lib/apt/lists/* \
    && docker-php-ext-install \
    pdo_mysql \
    zip \
    && echo ja_JP.UTF-8 UTF-8 > /etc/locale.gen && locale-gen \
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