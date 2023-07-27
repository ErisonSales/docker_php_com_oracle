FROM php:8.2-fpm
ARG uid=1000
ARG user=dev
RUN echo ˜-----------------------------------------˜
RUN echo $user
RUN echo ˜-----------------------------------------˜

# Instalação de pacotes e dependencias
RUN apt-get update && \
    apt-get -y --no-install-recommends install \
    git unzip libaio1 nano libldap2-dev unixodbc-dev npm imagemagick\
    && apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap && docker-php-ext-install pdo_mysql

RUN apt-get -y update \
    && apt-get install -y libicu-dev libpng-dev\ 
    && docker-php-ext-configure intl \
    && docker-php-ext-install intl \
    && docker-php-ext-enable sodium \
    && docker-php-ext-install gd

RUN apt-get update && apt-get install -y \
    libmagickwand-dev --no-install-recommends \
    && pecl install imagick \
    && docker-php-ext-enable imagick

# Config de Usuario docker
RUN useradd -G www-data,root -u $uid -d /home/dev/ $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user

# ####################################
# COMPOSER
# ####################################
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer

# ####################################
# SQLSRV
# ####################################
RUN pecl install sqlsrv pdo_sqlsrv
RUN docker-php-ext-enable sqlsrv && docker-php-ext-enable pdo_sqlsrv

# ####################################
# ORACLE INSTANTCLIENT
# ####################################
RUN mkdir /opt/oracle && cd /opt/oracle

ADD oracle/instantclient-basic-linux.x64-12.1.0.2.0.zip /opt/oracle
ADD oracle/instantclient-sdk-linux.x64-12.1.0.2.0.zip /opt/oracle

# Instalacao do instantclient
RUN  unzip /opt/oracle/instantclient-basic-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
    && unzip /opt/oracle/instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /opt/oracle \
    && ln -s /opt/oracle/instantclient_12_1/libclntsh.so.12.1 /opt/oracle/instantclient_12_1/libclntsh.so \
    && ln -s /opt/oracle/instantclient_12_1/libclntshcore.so.12.1 /opt/oracle/instantclient_12_1/libclntshcore.so \
    && ln -s /opt/oracle/instantclient_12_1/libocci.so.12.1 /opt/oracle/instantclient_12_1/libocci.so \
    && rm -rf /opt/oracle/*.zip

ENV LD_LIBRARY_PATH  /opt/oracle/instantclient_12_1:${LD_LIBRARY_PATH}

# Instalacao libs oracle php
RUN echo 'instantclient,/opt/oracle/instantclient_12_1/' | pecl install oci8 \
    && docker-php-ext-enable oci8 \
    && docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_12_1,12.1 \
    && docker-php-ext-install pdo_oci 

USER $user
WORKDIR "/var/www"