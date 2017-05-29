FROM php:7.1-fpm
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN apt-get update && apt-get install -y \
	libpng12-dev \
	git
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install zip
COPY src/ /var/www/html/
RUN cd /var/www/html && bin/grav install

