FROM ubuntu:14.04

RUN sed -i 's/http:\/\/archive\.ubuntu\.com\/ubuntu\//http:\/\/mirrors\.aliyun\.com\/ubuntu\//g' /etc/apt/sources.list

RUN echo 'deb http://ppa.launchpad.net/ondrej/php/ubuntu trusty main' > /etc/apt/sources.list.d/php.list

RUN apt-key adv --keyserver keyserver.ubuntu.com --recv-keys E5267A6C

RUN apt-get update && \
       apt-get install -y php7.0-cli \
       php7.0-curl \
       php7.0-mcrypt \
       php7.0-mysql \
       php7.0-gd \
       php7.0-xml \
       php7.0-dev \
       php7.0-zip \
       php7.0-xml \
       php7.0-opcache \
       php7.0-intl \
       php7.0-json \
       php7.0-pgsql \
       php7.0-imap \
       php-pear \
       libsasl2-dev \
       curl \
       git

RUN mkdir -p /usr/local/openssl/include/openssl/ && \
    ln -s /usr/include/openssl/evp.h /usr/local/openssl/include/openssl/evp.h && \
    mkdir -p /usr/local/openssl/lib/ && \
    ln -s /usr/lib/x86_64-linux-gnu/libssl.a /usr/local/openssl/lib/libssl.a && \
    ln -s /usr/lib/x86_64-linux-gnu/libssl.so /usr/local/openssl/lib/

RUN pecl install mongodb

RUN echo "extension=mongodb.so" > /etc/php/7.0/cli/conf.d/20-mongodb.ini && \
	echo "extension=mongodb.so" > /etc/php/7.0/mods-available/mongodb.ini

RUN curl -sS https://getcomposer.org/installer | php && \
    chmod +x composer.phar && mv composer.phar /usr/local/bin/composer

# Install nvm
ARG NODE_VERSION='6.1.0'
ENV NODE_VERSION ${NODE_VERSION}
RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.32.0/install.sh | bash && \
      . ~/.nvm/nvm.sh && \
      nvm install ${NODE_VERSION} && \
      nvm use ${NODE_VERSION} && \
      nvm alias ${NODE_VERSION} && \
      npm install -g gulp bower webpack \

RUN useradd -m -s /bin/bash docker

WORKDIR /home/docker

ADD app/* /home/docker
RUN chown -R docker:docker /home/docker

EXPOSE 9000
