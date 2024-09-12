FROM php:8.0-fpm
ARG WORKDIR=/var/www/html
ENV DOCUMENT_ROOT=${WORKDIR}
ENV DOMAIN=_
ENV CLIENT_MAX_BODY_SIZE=15M

# Install system dependencies
RUN apt-get update && apt-get install -y acl procps supervisor coreutils nginx

# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*


# Set working directory
WORKDIR $WORKDIR

RUN rm -Rf /var/www/* && \
mkdir -p /var/www/html

ADD src/index.php $WORKDIR/index.php
ADD src/supervisor/supervisord.conf /etc/supervisor/supervisord.conf

COPY src/entrypoint.sh /usr/local/bin/
RUN chmod +x /usr/local/bin/entrypoint.sh
RUN ln -s /usr/local/bin/entrypoint.sh /

RUN rm -rf /etc/nginx/conf.d/default.conf
RUN rm -rf /etc/nginx/sites-enabled/default
RUN rm -rf /etc/nginx/sites-available/default

RUN rm -rf /etc/nginx/nginx.conf

COPY src/nginx.conf /etc/nginx/nginx.conf
COPY src/default.conf /etc/nginx/conf.d/

RUN mkdir -p /var/cache/nginx

EXPOSE 8080
ENTRYPOINT ["entrypoint.sh"]