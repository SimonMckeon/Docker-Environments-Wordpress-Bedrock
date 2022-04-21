FROM wordpress:php8.0-fpm

# Install dependencies
RUN apt-get update && apt-get install less

# We can install composer in it's own container, but it's a bit of a nightmare
# trying to keep permissions in sync across containers. For ease of use,
# we will install composer directly inside the php container.
RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php composer-setup.php --install-dir=/usr/local/bin --filename=composer
RUN php -r "unlink('composer-setup.php');"

# Install WP-CLI
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar \
  && chmod +x wp-cli.phar \
  && mv wp-cli.phar /usr/local/bin/wp

# Install Xdebug
ENV XDEBUG_PORT 9001
ENV XDEBUG_IDEKEY docker
RUN pecl install xdebug && docker-php-ext-enable xdebug
COPY xdebug.ini "$PHP_INI_DIR/conf.d/"

ENTRYPOINT []
CMD ["php-fpm"]
