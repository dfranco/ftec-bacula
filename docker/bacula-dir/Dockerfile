FROM gpmidi/bacula-base:latest
MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>

# Install vchanger https://sourceforge.net/projects/vchanger/
RUN wget "https://master.dl.sourceforge.net/project/vchanger/RPM/vchanger-1.0.3-1.el7.x86_64.rpm?viasf=1" -O /tmp/vchangaer.rpm
RUN yum localinstall -y /tmp/vchanger.rpm

VOLUME ["/opt/bacula/etc"]

EXPOSE 9101/tcp

ENTRYPOINT ["bacula-dir"]

CMD ["-f", "-c", "/opt/bacula/etc/bacula-dir.conf"]
