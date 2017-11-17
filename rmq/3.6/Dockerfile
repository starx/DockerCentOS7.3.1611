FROM centos:7.3.1611
MAINTAINER Starx (mail@mrnepal.com)

RUN yum -y install epel-release && \
yum update -y && \
yum install -y erlang wget && \
rpm --import https://www.rabbitmq.com/rabbitmq-release-signing-key.asc && \
# Download and install
wget https://dl.bintray.com/rabbitmq/rabbitmq-server-rpm/rabbitmq-server-3.6.12-1.el7.noarch.rpm -O /tmp/rmq.rpm && \
yum install -y /tmp/rmq.rpm

RUN chkconfig rabbitmq-server on 
# Clean up
RUN rm -f /tmp/rmq.rpm && \
yum -y remove wget && \
rm -rf /var/cache/yum/* && \
yum clean all

COPY scripts /usr/share/container-scripts

EXPOSE 4369 5671 5672 25672
ENTRYPOINT ["/usr/share/container-scripts/docker-entrypoint"]
#CMD ["/usr/share/container-scripts/init-rmq"]
CMD ["rabbitmq-server"]