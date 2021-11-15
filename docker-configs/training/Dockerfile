FROM php:7.3-apache

ADD add_php.ini /usr/local/etc/php/conf.d/

RUN apt-get update -y && apt-get install -y libpng-dev libjpeg-dev zip unzip git libzip-dev
RUN docker-php-ext-configure gd \
		--with-jpeg-dir=/usr/lib/x86_64-linux-gnu/ \
	&& \
    docker-php-ext-install pdo pdo_mysql gd mbstring zip bcmath

# Install Postgre PDO
RUN apt-get install -y libpq-dev \
    && docker-php-ext-configure pgsql -with-pgsql=/usr/local/pgsql \
    && docker-php-ext-install pdo_pgsql pgsql

### composer
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# apacheの書き込み権限を修正する
RUN usermod -u 1000 www-data \
	&& groupmod -g 1000 www-data

RUN a2enmod rewrite

RUN apt-get install -y dialog locales

RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8


### Node
# gpg keys listed at https://github.com/nodejs/node
RUN apt-get install -y gnupg2
RUN set -ex \
  && for key in \
    4ED778F539E3634C779C87C6D7062848A1AB005C \
    B9E2F5981AA6E0CD28160D9FF13993A75599653C \
    94AE36675C464D64BAFA68DD7434390BDBE9B9C5 \
    B9AE9905FFD7803F25714661B63B535A4C206CA9 \
    77984A986EBC2AA786BC0F66B01FBB92821C587A \
    71DCFD284A79C3B38668286BC97EC7A07EDE3FC1 \
    FD3A5288F042B6850C66B31F09FE44734EB7990E \
    8FCCA13FEF1D0C2E91008E09770F7A9A5AE15600 \
    C4F0DFFF4E8C1A8236409D08E73BC641CC11F4C8 \
    DD8F2338BAE7501E3DD5AC78C273792F7D83545D \
    A48C2BEE680E841632CD4E44F07496B3EB3C1762 \
  ; do \
    gpg --no-tty --keyserver hkp://pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --no-tty --keyserver hkp://pgp.mit.edu:80 --recv-keys "$key" || \
    gpg --no-tty --keyserver hkp://keyserver.pgp.com:80 --recv-keys "$key" || \
    gpg --no-tty --keyserver hkp://ipv4.pool.sks-keyservers.net --recv-keys "$key" || \
    gpg --no-tty --keyserver hkp://p80.pool.sks-keyservers.net:80 --recv-keys "$key" ; \
  done

# node, npm, gulp
RUN apt-get install -y npm && \
    npm cache clean --force && \
    npm install n -g && \
    n stable && \
    # Remove old "nodejs, npm" installed first to avoid confusing.
    apt-get purge -y nodejs npm && \
    apt-get clean

