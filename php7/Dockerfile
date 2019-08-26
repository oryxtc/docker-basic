FROM php:7-fpm
COPY ./php.ini /usr/local/etc/php/

#update system timezone and  application timezone
RUN curl -o /etc/apt/sources.list.d/aliyun.list https://gist.githubusercontent.com/oryxtc/ba58ee12aebe92a4a8f1e9fb6baf69ea/raw/321c440618333ffb2fb2362ab939e9595c104cfb/aliyun.sources.list.jessie \
    && ln -sf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && echo "Asia/Shanghai" > /etc/timezone
RUN apt-get update && apt-get install -y \
        libzip-dev \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libpng-dev \
	&& pecl install redis-3.1.3 \
	&& docker-php-ext-install -j$(nproc) bcmath pdo pdo_mysql fileinfo zip \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-enable redis

#install composer
COPY --from=composer /usr/bin/composer /usr/bin/composer
