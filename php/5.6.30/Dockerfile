FROM centos:7.3.1611
MAINTAINER Starx (mail@mrnepal.com)

RUN yum -y install epel-release && \
yum update -y && \
yum -y groupinstall "Development Tools" && \
yum -y install \
	gcc-c++ \
	libxml2-devel \
	pkgconfig \
	openssl-devel \
	bzip2-devel \
	curl-devel \
	libpng-devel \
	libjpeg-devel \
	libXpm-devel \
	freetype-devel \
	gmp-devel \
	libmcrypt-devel \
	aspell-devel \
	recode-devel \
	autoconf \
	bison \
	re2c \
	libicu-devel \
	libxslt-devel \
	wget && \
# Download PHP and extract
wget https://secure.php.net/get/php-5.6.30.tar.xz/from/this/mirror -O /tmp/php.tar.xz && \
tar -xJf /tmp/php.tar.xz -C /tmp && \
cd /tmp/php-5.6.30 && \
# Build And Install
./buildconf --force && \
./configure \
	--prefix=/usr/local/php \
	--with-config-file-path=/usr/local/php/etc \
	--with-config-file-scan-dir=/usr/local/php/etc/conf.d \
	--enable-ftp \
	--enable-sockets \
	--enable-xml \
	--enable-bcmath \
	--with-bz2 \
	--with-curl \
	--enable-filter \
	--enable-fpm \
	--with-gd \
	--enable-gd-native-ttf \
	--with-freetype-dir \
	--with-jpeg-dir \
	--with-png-dir \
	--enable-intl \
	--enable-mbstring \
	--with-mcrypt \
	--with-mysql \
	--enable-mysqlnd \
	--with-mysql-sock=/var/lib/mysql/mysql.sock \
	--with-mysqli=mysqlnd \
	--with-pdo-mysql=mysqlnd \
	--disable-phpdbg \
	--enable-opcache \
	--with-openssl \
	--enable-simplexml \
	--enable-xmlreader \
	--enable-xmlwriter \
	--enable-zip \
	--with-zlib \
	--with-xmlrpc \
	--enable-soap \
	--disable-debug \
	--enable-calendar \
	--enable-wddx \
	--with-xsl \
	--with-gettext && \
cd /tmp/php-5.6.30 && \
make clean && \
make && \
make install && \
mkdir /usr/local/php/etc/conf.d/ && \
# Configuration PHP
useradd -r -s /bin/false php-fpm && \
cp /tmp/php-5.6.30/php.ini-production /usr/local/php/etc/php.ini && \
chmod 0644 /usr/local/php/etc/php.ini && \
mkdir /tmp/php_sessions && \
chown php-fpm:php-fpm /tmp/php_sessions && \
chmod 0744 /tmp/php_sessions && \
echo 'd /tmp/php_sessions 1744 php-fpm php-fpm\n\
R /tmp/php_sessions/*\n'\
>> /usr/lib/tmpfiles.d/php.conf && \
cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf && \
chmod 0644 /usr/local/php/etc/php-fpm.conf && \
sed -i 's/listen = 127.0.0.1:9000/listen = [::]:9000/' /usr/local/php/etc/php-fpm.conf && \
sed -i 's/user = nobody/user = php-fpm/' /usr/local/php/etc/php-fpm.conf && \
sed -i 's/group = nobody/group = php-fpm/' /usr/local/php/etc/php-fpm.conf && \
sed -i 's/;catch_workers_output/catch_workers_output/' /usr/local/php/etc/php-fpm.conf && \
sed -i 's|;php_admin_value\[error_log\]|php_admin_value[error_log]|' /usr/local/php/etc/php-fpm.conf && \
sed -i 's|/var/log/fpm-php.www.log|/var/log/php-fpm/fpm-php.error.log|' /usr/local/php/etc/php-fpm.conf && \
cp /tmp/php-5.6.30/sapi/fpm/php-fpm.service /usr/lib/systemd/system/php-fpm.service && \
sed -i 's|${prefix}|/usr/local/php|' /usr/lib/systemd/system/php-fpm.service && \
sed -i 's|${exec_prefix}|/usr/local/php|' /usr/lib/systemd/system/php-fpm.service && \
echo 'pathmunge /usr/local/php/bin' > /etc/profile.d/php.sh && \
ln -sfn /usr/local/php/bin/php /usr/bin/php && \
ln -sfn /usr/local/php/sbin/php-fpm /usr/sbin/php-fpm && \
mkdir /var/log/php-fpm && \
chown -hR php-fpm:php-fpm /var/log/php-fpm && \
# Clean up
yum -y --setopt=groupremove_leaf_only=1 groupremove 'Development Tools' && \
rm -f /tmp/php.tar.xz && \
rm -rf /tmp/php-5.6.30 && \
yum -y remove wget && \
rm -rf /var/cache/yum/* && \
yum clean all

COPY scripts /usr/share/container-scripts

WORKDIR /var/www/html

EXPOSE 9000
ENTRYPOINT ["/usr/share/container-scripts/docker-entrypoint"]
CMD ["php-fpm", "-F"]