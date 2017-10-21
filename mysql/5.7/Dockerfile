FROM centos:7.3.1611
MAINTAINER Starx (mail@mrnepal.com)

RUN yum update -y && \
yum -y install wget && \
wget https://dev.mysql.com/get/mysql57-community-release-el7-11.noarch.rpm && \
rpm -ivh mysql57-community-release-el7-11.noarch.rpm && \
yum update -y && \
yum install -y mysql mysql-server MySQL-python && \
mkdir /docker-mysql-data.d && \
# Clean ups
yum -y remove wget && \
rm -f mysql57-community-release-el7-11.noarch.rpm && \
rm -rf /var/cache/yum/* && \
yum clean all

VOLUME /var/lib/mysql

COPY scripts /usr/share/container-scripts

USER mysql

EXPOSE 3306
ENTRYPOINT ["/usr/share/container-scripts/docker-entrypoint"]
CMD ["/usr/share/container-scripts/init-mysqld"]