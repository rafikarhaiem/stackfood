FROM php:7.4-apache

WORKDIR /var/www/html/code2

RUN chown -R www-data:www-data /var/www/html/code2 \
    && a2enmod rewrite \
    && a2enmod headers

ENTRYPOINT apache2-foreground
