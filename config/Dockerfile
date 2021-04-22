FROM php:7.4-fpm

ARG uid=1000
ARG user=erison

# Instalação de dependencias
RUN apt-get update && \
    apt-get -y --no-install-recommends install \
    git unzip libaio1 nano libldap2-dev  \
    && apt-get autoclean && rm -rf /var/lib/apt/lists/*

# Instalação das configurações para LDAP e OCI8
RUN docker-php-ext-configure ldap --with-libdir=lib/x86_64-linux-gnu/ && \
    docker-php-ext-install ldap

RUN docker-php-ext-install pdo_mysql

# Config de Usuario docker
RUN useradd -G www-data,root -u $uid -d /home/user $user
RUN mkdir -p /home/$user/.composer && \
    chown -R $user:$user /home/$user


# Instalação composer
COPY --from=composer:latest /usr/bin/composer /usr/bin/composer


# ####################################
# INSTALL ORACLE INSTANTCLIENT
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
RUN echo 'instantclient,/opt/oracle/instantclient_12_1/' | pecl install oci8-2.2.0 \
    && docker-php-ext-enable oci8 \
    && docker-php-ext-configure pdo_oci --with-pdo-oci=instantclient,/opt/oracle/instantclient_12_1,12.1 \
    && docker-php-ext-install pdo_oci 

USER $user
WORKDIR "/var/www"