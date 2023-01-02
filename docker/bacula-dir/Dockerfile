FROM registry-1.docker.io/gpmidi/ftec-bacula:base-latest

MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>

VOLUME ["/opt/bacula/etc"]

EXPOSE 9101/tcp

ENTRYPOINT ["bacula-dir"]

CMD ["-f", "-c", "/opt/bacula/etc/bacula-dir.conf"]
