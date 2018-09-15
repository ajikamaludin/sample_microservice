FROM php:7.0-fpm

RUN apt-get update && apt-get -y install \
        git g++ libcurl4-gnutls-dev libicu-dev libmcrypt-dev libpq-dev libxml2-dev unzip zlib1g-dev \
    && docker-php-ext-install curl intl json mbstring mcrypt pdo pdo_mysql pdo_pgsql xml \
    && apt-get autoremove && apt-get autoclean \
    && rm -rf /var/lib/apt/lists/*

RUN pecl install -o -f redis \
    &&  rm -rf /tmp/pear \
    &&  docker-php-ext-enable redis

RUN echo 'date.timezone="Asia/Jakarta"' >> /usr/local/etc/php/conf.d/date.ini
RUN echo 'session.save_path = "/tmp"' >> /usr/local/etc/php/conf.d/session.ini

#consul
ENV CONSUL_TEMPLATE_VERSION 0.19.5

#RUN curl --retry 7 -Lso /tmp/consul-template.zip \ "https://releases.hashicorp.com/consul-template/${CONSUL_TEMPLATE_VERSION}/ consul-template_${CONSUL_TEMPLATE_VERSION}_linux_amd64.zip" \
COPY file/consul-template_0.19.5_linux_amd64.zip /tmp/consul-template.zip
RUN unzip /tmp/consul-template.zip -d /usr/local/bin \
    && rm /tmp/consul-template.zip

#container pilot
ENV CONTAINERPILOT_VERSION 3.0.0
ENV CONTAINERPILOT /etc/containerpilot.json

#RUN curl -soSL /tmp/containerpilot.tar.gz "https://github.com/joyent/containerpilot/releases/download/${CONTAINERPILOT_VERSION}/containerpilot-${CONTAINERPILOT_VERSION}.tar.gz" \
COPY file/containerpilot-3.0.0.tar.gz /tmp/containerpilot.tar.gz
RUN tar zxf /tmp/containerpilot.tar.gz -C /usr/local/bin \
    && rm /tmp/containerpilot.tar.gz

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer
RUN curl -sSL https://phar.phpunit.de/phpunit.phar -o /usr/bin/phpunit && chmod +x /usr/bin/phpunit

COPY config/ /etc

COPY scripts/ /usr/local/bin
RUN chmod +x /usr/local/bin/reload.sh

CMD [ "/usr/local/bin/containerpilot", \
    "php-fpm", \
        "--nodaemonize"]