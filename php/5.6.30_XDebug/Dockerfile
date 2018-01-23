FROM starx/centos7.3.1611-php-fpm:5.6.30
MAINTAINER Starx (mail@mrnepal.com)

RUN yum install -y wget autoconf && \
wget http://xdebug.org/files/xdebug-2.5.4.tgz -O /tmp/xdebug.tgz && \
tar -xvzf /tmp/xdebug.tgz -C /tmp && \
cd /tmp/xdebug-2.5.4 && \
# Build and Install
/usr/local/php/bin/phpize && \
./configure --with-php-config=/usr/local/php/bin/php-config && \
make && \
cp modules/xdebug.so /usr/local/php/lib/php/extensions/no-debug-non-zts-20131226/ && \
mkdir -p /var/log/xdebug_dumps && \
chown -hR php-fpm:php-fpm /var/log/xdebug_dumps && \
rm -f /tmp/xdebug.tgz && \
rm -rf /tmp/xdebug-2.5.4 && \
yum remove -y wget autoconf

ADD conf/xdebug.ini /usr/local/php/etc/conf.d/xdebug.ini

WORKDIR /var/www/html

EXPOSE 9000 9001
ENTRYPOINT ["/usr/share/container-scripts/docker-entrypoint"]
CMD ["php-fpm", "-F"]
