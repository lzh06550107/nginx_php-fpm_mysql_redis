FROM php:7.2-fpm
LABEL maintainer="gzp@goozp.com"


# set timezome
ENV TZ=Asia/Shanghai
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install package and PHP Core extensions
RUN apt-get update && apt-get install -y \
        git \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libpng-dev \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
        && docker-php-ext-install zip \
        && docker-php-ext-install pdo pdo_mysql mysqli \
        && docker-php-ext-install opcache \
        && docker-php-ext-install mysqli \
        && pecl install xdebug-2.6.0 \
        && pecl install redis \
        && rm -rf /tmp/pear \
        && docker-php-ext-enable xdebug redis \
        && rm -r /var/lib/apt/lists/*

# Copy extensions had downloaded，需要先下载源码包
# COPY ./pkg/redis.tgz /home/redis.tgz
# COPY ./pkg/cphalcon.tar.gz /home/cphalcon.tar.gz

# Install PECL extensions (Redis)
# RUN pecl install /home/redis.tgz && echo "extension=redis.so" > /usr/local/etc/php/conf.d/redis.ini

# Install Phalcon extensions
# RUN cd /home \
#     && tar -zxvf cphalcon.tar.gz \
#     && mv cphalcon-* phalcon \
#     && cd phalcon/build \
#     && ./install \
#     && echo "extension=phalcon.so" > /usr/local/etc/php/conf.d/phalcon.ini

# Install Composer
ENV COMPOSER_HOME /root/composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
ENV PATH $COMPOSER_HOME/vendor/bin:$PATH

RUN rm -f /home/redis.tgz

WORKDIR /data

# Write Permission,修改www-data用户uid
RUN usermod -u 1000 www-data
