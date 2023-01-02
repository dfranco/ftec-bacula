# get your key on: https://www.bacula.org/bacula-binary-package-download/
ARG BACULA_KEY
# Bacula version
ARG BACULA_VERSION
ARG EMAIL="paul+ftec-bacula@gpmidi.net"
ARG EL="8"

FROM centos:${EL}
MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>
ENV BACULA_KEY $BACULA_KEY
ENV BACULA_VERSION $BACULA_VERSION
ENV EMAIL $EMAIL
ENV EL $EL

RUN rpm --import https://www.bacula.org/downloads/Bacula-4096-Distribution-Verification-key.asc

COPY bacula-community.repo /etc/yum.repos.d/bacula-community.repo

RUN echo $BACULA_VERSION
RUN echo $BACULA_KEY

RUN sed -i "s/BACULA_VERSION/$(echo $BACULA_VERSION)/g" /etc/yum.repos.d/bacula-community.repo \
  && sed -i "s/BACULA_KEY/$(echo $BACULA_KEY)/g" /etc/yum.repos.d/bacula-community.repo \
  && sed -i "s/EL_VERSION/$(echo el$EL)/g" /etc/yum.repos.d/bacula-community.repo \
  && cat /etc/yum.repos.d/bacula-community.repo

RUN rpm -i https://download.postgresql.org/pub/repos/yum/reporpms/EL-${EL}-x86_64/pgdg-redhat-repo-latest.noarch.rpm

RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

# RUN dnf -y install postgresql \
RUN dnf update -y \
    && dnf -y install epel-release \
    && dnf config-manager --set-enabled PowerTools \
    && dnf -y install \
	postgresql10 \
	zip \
	bzip2 \
	mtx \
    bacula-postgresql-$BACULA_VERSION \
    --exclude=bacula-mysql \
    && dnf -y clean all && rm -rf /var/cache/yum

RUN for i in `ls /opt/bacula/bin`; do ln -s /opt/bacula/bin/$i /usr/sbin/$i; done 

# FIXME: Overwrite BACULA_KEY and in .repo
