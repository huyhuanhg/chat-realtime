#FROM php:7.2-fpm
#COPY composer.lock composer.json /var/www/
#COPY database /var/www/database
#WORKDIR /var/www
#RUN apt-get update && apt-get -y install git && apt-get -y install zip
#RUN php -r "copy(<https://getcomposer.org/installer<, <composer-setup.php<);"
#   && php -r "if (hash_file(<SHA384<, <composer-setup.php<) === <a5c698ffe4b8e849a443b120cd5ba38043260d5c4023dbf93e1558871f1f07f58274fc6f4c93bcfd858c6bd0775cd8d1<) { echo <Installer verified<; } else { echo <Installer corrupt<; unlink(<composer-setup.php<); } echo PHP_EOL;"
#   && php composer-setup.php
#   && php -r "unlink(<composer-setup.php<);"
#   && php composer.phar install --no-dev --no-scripts
#   && rm composer.phar
#COPY . /var/www
#RUN chown -R www-data:www-data
#       /var/www/storage
#       /var/www/bootstrap/cache
#
#RUN php artisan cache:clear
#RUN php artisan optimize
#RUN  apt-get install -y libmcrypt-dev
#       libmagickwand-dev --no-install-recommends
#       && pecl install mcrypt-1.0.2
#       && docker-php-ext-install pdo_mysql
#       && docker-php-ext-enable mcrypt
#
#RUN mv .env.prod .env
#RUN php artisan optimize

FROM php:7.4-fpm

# Copy composer.lock and composer.json
COPY composer.lock composer.json /var/www/

# Set working directory
WORKDIR /var/www

# Install dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpng-dev \
    libzip-dev\
    libonig-dev \
    libjpeg62-turbo-dev \
    libfreetype6-dev \
    locales \
    zip \
    jpegoptim optipng pngquant gifsicle \
    vim \
    unzip \
    git \
    curl


# Clear cache
RUN apt-get clean && rm -rf /var/lib/apt/lists/*

# Install extensions
RUN docker-php-ext-install pdo_mysql mbstring zip exif pcntl
RUN docker-php-ext-configure gd --with-freetype=/usr/include/ --with-jpeg=/usr/include/
RUN docker-php-ext-install gd

# Install composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
 # && composer install


# RUN composer install --optimize-autoloader --no-dev
#    php artisan migrate; \
#    php artisan db:seed; \
#    php artisan cache:clear; \
#RUN php artisan config:cache
#    php artisan route:cache; \
#    php artisan storage:link; \
#RUN chown -R www-data:www-data /var/www

# Add user for laravel application
RUN groupadd -g 1000 www
RUN useradd -u 1000 -ms /bin/bash -g www www

# Copy existing application directory contents
COPY . /var/www

# Copy existing application directory permissions
COPY --chown=www:www . /var/www

# Change current user to www
USER www

# Expose port 9000 and start php-fpm server
EXPOSE 9000
CMD ["php-fpm"]
