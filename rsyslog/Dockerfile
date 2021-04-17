# FROM registry.access.redhat.com/rhel7:latest
FROM registry.access.redhat.com/ubi7/ubi
# FROM registry.stage.redhat.io/rhel7/rhel:7.9

ADD rsyslog.conf /etc/rsyslog.conf
# RUN yum-config-manager --enable rhel-7-server-eus-rpms && yum repolist enabled
# RUN subscription-manager attach --auto
# RUN INSTALL_PKGS="\
# rsyslog \
# rsyslog-gnutls \
# rsyslog-gssapi \
# rsyslog-mysql \
# rsyslog-pgsql \
# rsyslog-relp \
# " && yum -y install $INSTALL_PKGS && rpm -V --nosize --nofiledigest --nomtime --nomode $INSTALL_PKGS && yum clean all

RUN yum update -y \
 && yum -y install rsyslog \
 && yum clean all \
 && rm -rf /var/cache/yum
 #&& sudo chmod -R ugo+rw,+X /etc/rsyslog.conf /var/log/ /var/run/

VOLUME /var/log/
WORKDIR /var/log/

EXPOSE 1601
ENTRYPOINT [ "/usr/sbin/rsyslogd" ]
CMD [ "-4", "-n", "-f", "/etc/rsyslog.conf", "-i", "/var/run/rsyslog.pid" ]

