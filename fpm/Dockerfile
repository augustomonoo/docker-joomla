FROM php:5.6-fpm
MAINTAINER Michael Babker <michael.babker@joomla.org> (@mbabker)

# Install PHP extensions
RUN apt-get update && apt-get install -y libpng12-dev libjpeg-dev zip unzip && rm -rf /var/lib/apt/lists/* \
	&& docker-php-ext-configure gd --with-png-dir=/usr --with-jpeg-dir=/usr \
	&& docker-php-ext-install gd
RUN docker-php-ext-install mysqli

VOLUME /var/www/html

# Define Joomla version and expected SHA1 signature
ENV JOOMLA_VERSION 1.2.0rc5
ENV JOOMLA_SHA1 4567d750c1fd92be5fddbf093a5d57769df443eb

# Download package and extract to web volume
RUN curl -o joomla.zip -SL https://github.com/joomlagovbr/joomla-3.x/archive/${JOOMLA_VERSION}.zip \
	&& echo "$JOOMLA_SHA1 *joomla.zip" | sha1sum -c - \
	&& mkdir -p /usr/src \
	&& unzip joomla.zip -d /tmp/ \
	&& mv /tmp/joomla-3.x-${JOOMLA_VERSION} /usr/src/joomla \
	&& rm joomla.zip \
	&& chown -R www-data:www-data /usr/src/joomla

# Copy init scripts and custom .htaccess
COPY docker-entrypoint.sh /entrypoint.sh
COPY makedb.php /makedb.php

ENTRYPOINT ["/entrypoint.sh"]
CMD ["php-fpm"]
