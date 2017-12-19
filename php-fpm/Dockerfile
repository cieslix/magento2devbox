FROM php:7.0-fpm
RUN apt-get update && apt-get install -y \
        libfreetype6-dev \
        libjpeg62-turbo-dev \
        libmcrypt-dev \
        libpng12-dev \
        libssl-dev \
        libxml2-dev \
        zlib1g-dev \
        zlib1g-dev libicu-dev g++ \
    	libpcre3 \
        cron git \
        imagemagick libmagickwand-dev libmagickcore-dev \
        libxslt1-dev libxslt1.1 \
        curl libcurl3 libcurl3-dev \
        --no-install-recommends \
    && docker-php-ext-install -j$(nproc) opcache session mbstring intl iconv mcrypt pdo pdo_mysql dom mysqli curl json zip soap xml xsl \
    && docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ \
    && docker-php-ext-install -j$(nproc) gd bcmath

# Install APCu
RUN pecl install imagick
RUN pecl install redis

RUN docker-php-ext-enable imagick
RUN docker-php-ext-enable redis

RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN curl -L http://pestle.pulsestorm.net/pestle.phar > /usr/local/bin/pestle.phar
RUN chmod a+x /usr/local/bin/pestle.phar
RUN curl -sS https://files.magerun.net/n98-magerun2.phar > /usr/local/bin/n98-magerun2.phar
RUN chmod a+x /usr/local/bin/n98-magerun2.phar
RUN curl -L https://github.com/staempfli/magento2-code-generator/releases/download/1.8/mg2-codegen.phar > /usr/local/bin/m2-codegen.phar
RUN chmod a+rwx /usr/local/bin/m2-codegen.phar

RUN usermod -u 1000 www-data
RUN usermod -G staff www-data
RUN chown www-data:www-data /var/www
 
ADD m2-crontab /tmp/m2-crontab
RUN crontab -u www-data /tmp/m2-crontab

WORKDIR "/var/www/magento2"
