###############################################################################
# PHP-7.3 optimized image
#
#

# Building extensions first
FROM php:7.3-cli-alpine as extensions_source

MAINTAINER "Gustavo Antão <gustavo.antao@gmail.com>"

ENV EXT_DIR=/usr/src/php/ext
RUN mkdir -p ${EXT_DIR}

RUN apk add --update php

# PHP MBSTRING
RUN docker-php-ext-install mbstring


# PHP-ZIP
RUN apk add --update zlib-dev libzip-dev
RUN docker-php-ext-install zip

# PHP-INTL
RUN apk add --update icu-dev
RUN docker-php-ext-configure intl && docker-php-ext-install intl

# PHP-GD
RUN apk add --update libgd libpng-dev
RUN docker-php-ext-configure gd && docker-php-ext-install gd

# PHP-IMAP
RUN apk add --update imap-dev
RUN docker-php-ext-configure imap && docker-php-ext-install imap

# PHP-XML
RUN apk add --no-cache -X http://dl-cdn.alpinelinux.org/alpine/edge/testing libxml++-dev
RUN docker-php-ext-configure xml && docker-php-ext-install xml

# PHP-MYSQLi
RUN docker-php-ext-configure mysqli && docker-php-ext-install mysqli

# PHP-JSON
RUN docker-php-ext-configure json && docker-php-ext-install json

# PHP-BCMATH
RUN docker-php-ext-configure bcmath && docker-php-ext-install bcmath

# PHP-IGBINARY
RUN cd $EXT_DIR \
    && curl -fsSL https://github.com/igbinary/igbinary7/archive/master.tar.gz -o igbinary.tar.gz \
    && mkdir -p igbinary \
    && tar -xf igbinary.tar.gz -C igbinary --strip-components=1 \
    && rm igbinary.tar.gz \
    && docker-php-ext-configure igbinary && docker-php-ext-install igbinary

# PHP-BZ2
RUN docker-php-ext-configure bz2 && docker-php-ext-install bz2    

# PHP-PDO
RUN docker-php-ext-configure pdo && docker-php-ext-install pdo
# PHP-PDO
RUN docker-php-ext-configure pdo_mysql && docker-php-ext-install pdo_mysql

# PHP-LDAP
RUN apk add --update openldap-dev
RUN docker-php-ext-configure ldap && docker-php-ext-install ldap


# Next, build a clean php-fpm-alpine image and just copy the libs to new image
FROM php:7.3-fpm-alpine

ENV EXT_DIR=/usr/local/lib/php/extensions/no-debug-non-zts-20180731/
ENV LIB_DIR=/usr/lib

COPY --from=extensions_source \
${LIB_DIR}/libzip.so \
${LIB_DIR}/libstdc++.so.6 \
${LIB_DIR}/libicudata.so \
${LIB_DIR}/libicui18n.so \
${LIB_DIR}/libicuio.so \
${LIB_DIR}/libicutest.so \
${LIB_DIR}/libicutu.so \
${LIB_DIR}/libicuuc.so \
${LIB_DIR}/libgcc_s.so.1 \
${LIB_DIR}/libpng.so \
${LIB_DIR}/libc-client.so \
${LIB_DIR}/libbz2.so \
${LIB_DIR}/libldap.so \
${LIB_DIR}/libldap_r-2.4.so.2 \
${LIB_DIR}/libldap_r.so \
${LIB_DIR}/liblber.so \
${LIB_DIR}/libsasl2.so ${LIB_DIR}/

COPY --from=extensions_source \
${EXT_DIR}/mbstring.so \
${EXT_DIR}/zip.so \
${EXT_DIR}/intl.so \
${EXT_DIR}/gd.so \
${EXT_DIR}/imap.so \
${EXT_DIR}/xml.so \
${EXT_DIR}/mysqli.so \
${EXT_DIR}/json.so \
${EXT_DIR}/bcmath.so \
${EXT_DIR}/igbinary.so \
${EXT_DIR}/bz2.so \
${EXT_DIR}/pdo.so \
${EXT_DIR}/pdo_mysql.so \
${EXT_DIR}/ldap.so ${EXT_DIR}/


RUN ln -s ${LIB_DIR}/libzip.so ${LIB_DIR}/libzip.so.5 \
&& ln -s ${LIB_DIR}/libbz2.so ${LIB_DIR}/libbz2.so.1 \
&& ln -s ${LIB_DIR}/libicuio.so ${LIB_DIR}/libicuio.so.64 \
&& ln -s ${LIB_DIR}/libicui18n.so ${LIB_DIR}/libicui18n.so.64 \
&& ln -s ${LIB_DIR}/libicuuc.so ${LIB_DIR}/libicuuc.so.64 \
&& ln -s ${LIB_DIR}/libicudata.so ${LIB_DIR}/libicudata.so.64 \
&& ln -s ${LIB_DIR}/libldap.so ${LIB_DIR}/libldap-2.4.so.2 \
&& ln -s ${LIB_DIR}/liblber.so ${LIB_DIR}/liblber-2.4.so.2 \
&& ln -s ${LIB_DIR}/libsasl2.so ${LIB_DIR}/libsasl2.so.3 \
&& ln -s ${LIB_DIR}/libpng.so ${LIB_DIR}/libpng16.so \
&& ln -s ${LIB_DIR}/libpng.so ${LIB_DIR}/libpng16.so.16 \
&& ln -s ${LIB_DIR}/libc-client.so ${LIB_DIR}/libc-client.so.1 \
&& docker-php-ext-enable zip \
&& docker-php-ext-enable mbstring \
&& docker-php-ext-enable intl \
&& docker-php-ext-enable gd \
&& docker-php-ext-enable imap \
&& docker-php-ext-enable xml \
&& docker-php-ext-enable mysqli \
&& docker-php-ext-enable json \
&& docker-php-ext-enable bcmath \
&& docker-php-ext-enable igbinary \
&& docker-php-ext-enable bz2 \
&& docker-php-ext-enable pdo \
&& docker-php-ext-enable pdo_mysql \
&& docker-php-ext-enable ldap

# Making php composer and git commands available
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');" && \
    php -r "if (hash_file('sha384', 'composer-setup.php') === 'a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;" && \
    php composer-setup.php && \
    php -r "unlink('composer-setup.php');" && \
    mv composer.phar /usr/local/bin/composer

RUN apk add --no-cache git

# Cleaning up image
RUN rm -rf /var/cache/apk/* /tmp/* /usr/share/man /usr/local/lib/php/doc/*