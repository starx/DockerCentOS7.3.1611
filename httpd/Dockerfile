FROM centos:7.3.1611
MAINTAINER Starx (mail@mrnepal.com)

RUN yum -y --setopt=tsflags=nodocs update && \
    yum -y --setopt=tsflags=nodocs install httpd mod_ssl mod_ldap && \
    yum clean all

COPY scripts /usr/share/container-scripts

EXPOSE 80 443
ENTRYPOINT ["/usr/share/container-scripts/docker-entrypoint"]
CMD ["/usr/share/container-scripts/run-httpd.sh"]