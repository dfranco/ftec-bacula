ARG EL_VERSION="8"
FROM rockylinux/rockylinux:${EL_VERSION}
MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>
# Enterprise Linux Version (eg 7, 8, 9)
ARG EL_VERSION
# get your key on: https://www.bacula.org/bacula-binary-package-download/
ARG BACULA_KEY
# Bacula version (ex 13.0.1)
ARG BACULA_VERSION
# Contact Email for image builder
ARG EMAIL=admin@domain.local

RUN rpm --import "https://www.bacula.org/downloads/Bacula-4096-Distribution-Verification-key.asc"

COPY bacula-community.repo /etc/yum.repos.d/bacula-community.repo
RUN sed -i "s/BACULA_VERSION/$BACULA_VERSION/g" /etc/yum.repos.d/bacula-community.repo \
 && sed -i "s/BACULA_KEY/$BACULA_KEY/g" /etc/yum.repos.d/bacula-community.repo \
 && sed -i "s/EL_VERSION/$EL_VERSION/g" /etc/yum.repos.d/bacula-community.repo

RUN dnf -y install epel-release \
 && dnf module enable -y postgresql:13 \
 && dnf --enablerepo=powertools install perl-IPC-Run -y \
 && dnf update -qy \
 && dnf -qy install \
      postgresql-server \
      vim \
      zip \
      bzip2 \
      mtx \
      bacula-postgresql-$BACULA_VERSION \
      --exclude=bacula-mysql \
 && dnf -qy clean all && rm -rf /var/cache/yum

RUN for i in `ls /opt/bacula/bin`; do ln -s /opt/bacula/bin/$i /usr/sbin/$i; done 

# FIXME: Overwrite BACULA_KEY and in .repo
