FROM php:7.4-apache

WORKDIR /var/www/html/code1

RUN apt-get update && apt-get install -y \
    libpng-dev \
    zlib1g-dev \
    libxml2-dev \
    libzip-dev \
    libonig-dev \
    zip \
    curl \
    unzip \
    libmagickwand-dev --no-install-recommends \
    supervisor \
    ghostscript \
    libfreetype6-dev \
    libjpeg62-turbo-dev \
    libpng-dev \
    libc-client-dev libkrb5-dev \
    libpq-dev

RUN curl -sL https://deb.nodesource.com/setup_16.x | bash -
RUN apt-get install -y nodejs
RUN docker-php-ext-configure gd --with-freetype --with-jpeg
RUN docker-php-ext-install -j$(nproc) gd
RUN docker-php-ext-install pdo
RUN docker-php-ext-install pdo_mysql
RUN docker-php-ext-install mysqli
RUN docker-php-ext-install xml
RUN docker-php-ext-install zip
RUN docker-php-ext-install mbstring


RUN mkdir -p /usr/src/php/ext/imagick
RUN curl -fsSL https://github.com/Imagick/imagick/archive/06116aa24b76edaf6b1693198f79e6c295eda8a9.tar.gz | tar xvz -C "/usr/src/php/ext/imagick" --strip 1
RUN docker-php-source delete
RUN apt-get clean
RUN rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

RUN sed -i '/disable ghostscript format types/,+6d' /etc/ImageMagick-6/policy.xml

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

RUN chown -R www-data:www-data /var/www/html/code1 \
    && a2enmod rewrite \
    && a2enmod headers

RUN curl -sS https://dl.yarnpkg.com/debian/pubkey.gpg | apt-key add - \
    && echo "deb https://dl.yarnpkg.com/debian/ stable main" > /etc/apt/sources.list.d/yarn.list \
    && apt-get update \
    && apt-get install -y yarn

RUN apt-get update; \
    apt-get install -y libmagickwand-dev; \
    pecl install imagick; \
    docker-php-ext-enable imagick;

ENTRYPOINT apache2-foreground

