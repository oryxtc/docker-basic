FROM php:5.5-fpm
COPY ./pecl /usr/local/pecl
RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
    && curl -o /etc/apt/sources.list https://gist.githubusercontent.com/oryxtc/5084379afcceada2c22278ff1b0ad420/raw/635920dcffa597ea968300f0c92ec7fc5c5ee4a7/debian-china \
	&& cp /usr/share/zoneinfo/Asia/Shanghai /etc/localtime
RUN apt-get update && apt-get install -y \
		libssl-dev \
		zlib1g-dev \
		libfreetype6-dev \
		libjpeg62-turbo-dev \
		libmcrypt-dev \
		libpng-dev \
		librdkafka-dev \
		libcurl4-openssl-dev \
	&& pecl install /usr/local/pecl/redis-3.1.5.tgz \
	&& pecl install /usr/local/pecl/xdebug-2.5.5.tgz \
	&& pecl install /usr/local/pecl/rdkafka-3.0.5.tgz \
	&& pecl install /usr/local/pecl/msgpack-0.5.7.tgz \
	&& pecl install /usr/local/pecl/yar-1.2.4.tgz \
    && cp -r /usr/include/x86_64-linux-gnu/curl/ /usr/include/curl/ \
    && mkdir -p yar-1.2.4 \
    && tar -zxvf /usr/local/pecl/yar-1.2.4.tgz -C yar-1.2.4 --strip-components=1 \
    && rm /usr/local/pecl/yar-1.2.4.tgz \
    && ( \
            cd yar-1.2.4 \
            && phpize \
            && ./configure --enable-msgpack \
            && make -j "$(nproc)" \
            && make install \
    ) \
    && rm -r yar-1.2.4 \
	&& mkdir -p yaf-2.2.9 \
	&& tar -zxvf /usr/local/pecl/yaf-2.2.9.tgz -C yaf-2.2.9 --strip-components=1 \
	&& rm /usr/local/pecl/yaf-2.2.9.tgz \
	&& ( \
            cd yaf-2.2.9 \
            && phpize \
            && ./configure \
            && make -j "$(nproc)" \
            && make install \
	) \
	&& rm -r yaf-2.2.9 \
	&& docker-php-ext-install -j$(nproc) bcmath pdo pdo_mysql fileinfo zip sockets \
	&& docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
	&& docker-php-ext-install -j$(nproc) gd \
	&& docker-php-ext-enable redis rdkafka yaf msgpack yar xdebug sockets