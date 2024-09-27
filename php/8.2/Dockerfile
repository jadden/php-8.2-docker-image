FROM php:8.2-alpine

ARG BUILD_DATE
ARG VCS_REF

ENV IMAGE_USER=php
ENV IMAGE_VERSION=$IMAGE_VERSION
ENV HOME=/home/$IMAGE_USER
ENV COMPOSER_HOME=$HOME/.composer
ENV PATH=$HOME/.yarn/bin:$PATH
ENV GOSS_VERSION="0.3.16"
ENV NODE_VERSION=14
ENV NPM_VERSION=8
ENV YARN_VERSION=latest
ENV PHP_VERSION=8.2

WORKDIR /tmp

COPY ./php/scripts/alpine/*.sh /tmp/
COPY --from=composer:2 /usr/bin/composer /usr/bin/composer
COPY --from=mhart/alpine-node:14 /usr/bin/node /usr/bin/
COPY --from=mhart/alpine-node:14 /usr/lib/libgcc* /usr/lib/libstdc* /usr/lib/* /usr/lib/

RUN apk update --no-cache \
  && chmod +x /tmp/*.sh \
  && adduser -D $IMAGE_USER \
  && mkdir -p /var/www/html \
  && apk add --update --no-cache bash \
  && bash ./packages.sh \
  && bash ./extensions.sh \
  && bash ./nodeyarn.sh \
  && rm -rf ~/.composer/cache/* \
  && chown -R $IMAGE_USER:$IMAGE_USER /var/www $HOME \
  && echo "$IMAGE_USER  ALL = ( ALL ) NOPASSWD: ALL" >> /etc/sudoers \
  && curl -fsSL https://goss.rocks/install | GOSS_VER=v${GOSS_VERSION} sh \
  && bash ./cleanup.sh

USER $IMAGE_USER

WORKDIR /var/www/html

CMD ["php", "-a"]
