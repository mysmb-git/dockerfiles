# alpine:latest at 2019-01-04T21:27:39IST
FROM alpine@sha256:46e71df1e5191ab8b8034c5189e325258ec44ea739bba1e5645cff83c9048ff1

LABEL maintainer="Riddhesh Sanghvi <riddheshsanghvi96@gmail.com>"

# Environments
ENV TIMEZONE                 Asia/Kolkata
ENV PHP_MEMORY_LIMIT         512M
ENV MAX_UPLOAD               50M
ENV PHP_MAX_FILE_UPLOAD      200
ENV PHP_MAX_POST             100M
ENV PHP_INI_DIR              /etc/php7/php.ini

# PHP-CLI installation inspired from https://github.com/bravist/php-cli-alpine-aliyun-app/blob/master/Dockerfile
# https://github.com/matriphe/docker-alpine-php/blob/master/7.0/FPM/Dockerfile

RUN apk update && \
	apk upgrade && \
	apk add \
		tzdata \
		php7 \
		php7-apcu \
		php7-bcmath \
		php7-bz2 \
		php7-ctype \
		php7-curl \
		php7-dev \
		php7-dom \
		php7-exif \
		php7-fileinfo \
		php7-gd \
		php7-iconv \
		php7-intl \
		php7-json \
		php7-mbstring\
		php7-mcrypt \
		php7-memcached \
		php7-mysqli \
		php7-mysqlnd \
		php7-opcache \
		php7-openssl \
		php7-pcntl \
		php7-pdo \
		php7-pdo_mysql \
		php7-pdo_pgsql \
		php7-phar \
		php7-posix \
		php7-redis\
		php7-session \
		php7-simplexml \
		php7-tokenizer \
		php7-xml \
		php7-xmlreader \
		php7-xmlwriter \
		php7-xsl \
		php7-zip \
		php7-zlib \
		php7-zmq && \
	cp /usr/share/zoneinfo/${TIMEZONE} /etc/localtime && \
	echo "${TIMEZONE}" > /etc/timezone && \
	apk del tzdata && \
	rm -rf /var/cache/apk/*

# https://github.com/docker-library/php/issues/240
# https://gist.github.com/guillemcanal/be3db96d3caa315b4e2b8259cab7d07e
# https://forum.alpinelinux.org/forum/installation/php-iconv-issue
# https://github.com/gliderlabs/docker-alpine/issues/157#issuecomment-200860301

RUN apk add --no-cache --repository http://dl-cdn.alpinelinux.org/alpine/edge/testing gnu-libiconv 
ENV LD_PRELOAD /usr/lib/preloadable_libiconv.so php

RUN rm -rf /var/cache/apk/*

# Set environments
RUN sed -i "s|;*date.timezone =.*|date.timezone = ${TIMEZONE}|i" "$PHP_INI_DIR" && \
	sed -i "s|;*memory_limit =.*|memory_limit = ${PHP_MEMORY_LIMIT}|i" "$PHP_INI_DIR" && \
	sed -i "s|;*upload_max_filesize =.*|upload_max_filesize = ${MAX_UPLOAD}|i" "$PHP_INI_DIR" && \
	sed -i "s|;*max_file_uploads =.*|max_file_uploads = ${PHP_MAX_FILE_UPLOAD}|i" "$PHP_INI_DIR" && \
	sed -i "s|;*post_max_size =.*|post_max_size = ${PHP_MAX_POST}|i" "$PHP_INI_DIR" && \
	sed -i "s|;*cgi.fix_pathinfo=.*|cgi.fix_pathinfo= 0|i" "$PHP_INI_DIR"

# Update php config
RUN mkdir -p "/etc/php7/conf.d" && \
	echo "memory_limit=-1" > "/etc/php7/conf.d/memory-limit.ini" && \
	echo "date.timezone=${TIMEZONE}" > "/etc/php7/conf.d/date_timezone.ini"

CMD ["php", "-a"]
