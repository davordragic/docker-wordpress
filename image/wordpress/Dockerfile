FROM wordpress:php7.4-apache

RUN apt-get update && \
	apt-get install -y  --no-install-recommends ssl-cert && \
	rm -r /var/lib/apt/lists/* && \
	a2enmod ssl && \
	a2ensite default-ssl

# Install packages under Debian
RUN apt-get update && \
    apt-get -y install git
# Install XDebug from source as described here:
# https://xdebug.org/docs/install
# Available branches of XDebug could be seen here:
# https://github.com/xdebug/xdebug/branches
RUN cd /tmp && \
    git clone git://github.com/xdebug/xdebug.git && \
    cd xdebug && \
    git checkout xdebug_3_0 && \
    phpize && \
    ./configure --enable-xdebug && \
    make && \
    make install && \
    rm -rf /tmp/xdebug
# Copy xdebug.ini to /usr/local/etc/php/conf.d/
COPY xdebug.ini /usr/local/etc/php/conf.d/
# Since this Dockerfile extends the official Docker image `wordpress`,
# and since `wordpress`, in turn, extends the official Docker image `php`,
# the helper script docker-php-ext-enable (defined for image `php`)
# works here, and we can use it to enable xdebug:
RUN docker-php-ext-enable xdebug

#COPY 000-default.conf /etc/apache2/sites-available/
#COPY php.ini /usr/local/etc/php/php.ini
#
#RUN apt update
#RUN apt install nano sudo
#
#RUN groupadd -g 1000 app
#RUN useradd -g 1000 -u 1000 -s /bin/bash app
#RUN usermod -aG sudo app
#RUN echo '%sudo ALL=(ALL) NOPASSWD:ALL' >> /etc/sudoers

#USER app
EXPOSE 80
EXPOSE 443