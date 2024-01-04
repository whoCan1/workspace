ARG PHP_VERSION
FROM ${PHP_VERSION}

ARG TZ

# 设置时区
RUN apt-get update && apt-get install -y tzdata && rm -r /var/lib/apt/lists/* && apt-get clean
ENV TZ ${TZ}
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && \
    apt-get install -y --no-install-recommends libzip-dev && \
    rm -r /var/lib/apt/lists/* && \
    docker-php-ext-install -j$(nproc) zip

RUN apt-get update && \
    apt-get install -y --no-install-recommends libwebp-dev libjpeg-dev libpng-dev libfreetype6-dev && \
    rm -r /var/lib/apt/lists/* && \
    docker-php-ext-configure gd --with-jpeg=/usr/include --with-freetype=/usr/include/ && \
    docker-php-ext-install -j$(nproc) gd

RUN docker-php-ext-install -j$(nproc) bcmath pdo_mysql fileinfo

RUN pecl install -o -f redis && rm -rf /tmp/pear && docker-php-ext-enable redis

RUN pecl install -o -f swoole && rm -rf /tmp/pear && docker-php-ext-enable swoole

# Install composer and change it's cache home
RUN curl -o /usr/bin/composer https://mirrors.aliyun.com/composer/composer.phar \
    && chmod +x /usr/bin/composer

RUN composer config -g repo.packagist composer https://mirrors.aliyun.com/composer/

RUN apt-get clean

RUN usermod -u 1000 www-data && groupmod -g 1000 www-data

WORKDIR /www