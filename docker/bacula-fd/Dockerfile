FROM gpmidi/bacula-base:latest
MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>

VOLUME ["/opt/bacula/etc"]

EXPOSE 9102/tcp

ENTRYPOINT ["bacula-fd"]

CMD ["-f", "-c", "/opt/bacula/etc/bacula-fd.conf"]
