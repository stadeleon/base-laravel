FROM  webdevops/php-dev:7.4

MAINTAINER Rocketleap Backend Team
# Fix debconf warnings upon build
ARG DEBIAN_FRONTEND=noninteractive
ARG PUID=1000
ENV PROVISION_CONTEXT "development"
ENV PUID ${PUID}

#####################################
RUN apt-get update -qq && apt-get install -qq -y \
    supervisor net-tools vim wget htop \
    --no-install-recommends \
 && mkdir /conf \
 && mkdir /logs \
 && mkdir /shared \
 && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

#####################################

# Deploy scripts/configurations
COPY etc/             /opt/docker/etc/
RUN ln -sf /opt/docker/etc/cron/crontab /etc/cron.d/docker-boilerplate \
    && chmod 0644 /opt/docker/etc/cron/crontab \
    && echo >> /opt/docker/etc/cron/crontab \
    && ln -sf /opt/docker/etc/php/development.ini /opt/docker/etc/php/php.ini

# Configure volume/workdir
WORKDIR /app/

COPY --from=composer:latest /usr/bin/composer /usr/local/bin/composer
COPY . ./

RUN rm -rf /root/.composer \
&& echo 'alias ll="ls -alF --color=auto"' >> ~/.bashrc

USER ${PUID}
RUN composer global require hirak/prestissimo laravel/installer
