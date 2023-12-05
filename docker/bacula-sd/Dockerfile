FROM gpmidi/bacula-base:latest
MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>

RUN yum install -y bacula-cloud-storage --exclude=bacula-mysql

RUN for i in `ls /opt/bacula/bin`; do if test -z /usr/sbin/$i; then ln -s /opt/bacula/bin/$i /usr/sbin/$i; fi; done 

# Install vchanger https://sourceforge.net/projects/vchanger/
RUN curl "https://master.dl.sourceforge.net/project/vchanger/RPM/vchanger-1.0.3-1.el7.x86_64.rpm?viasf=1" -o /tmp/vchangaer.rpm
RUN yum localinstall -y /tmp/vchanger.rpm

VOLUME ["/opt/bacula/etc"]

EXPOSE 9103/tcp

ENTRYPOINT ["bacula-sd"]

CMD ["-f", "-c", "/opt/bacula/etc/bacula-sd.conf"]
