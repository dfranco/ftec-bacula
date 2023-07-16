ARG EL_VERSION="8"
FROM rockylinux/rockylinux:${EL_VERSION}
MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>
# Enterprise Linux Version (ex 7, 8, 9)
ARG EL_VERSION
# get your key on: https://www.bacula.org/bacula-binary-package-download/
ARG BACULA_KEY
# Bacula version (ex 13.0.1)
ARG BACULA_VERSION
# Contact Email for image builder
ARG EMAIL=paul+ftec-bacula@gpmidi.net

# Optional Debug stuff
#RUN echo "Bacula Version: $BACULA_VERSION"
#RUN echo "Key: $BACULA_KEY"
#RUN echo "EL Version: $EL_VERSION"

RUN rpm --import "https://www.bacula.org/downloads/Bacula-4096-Distribution-Verification-key.asc"

COPY bacula-community.repo /etc/yum.repos.d/bacula-community.repo
RUN sed -i "s/BACULA_VERSION/$BACULA_VERSION/g" /etc/yum.repos.d/bacula-community.repo \
 && sed -i "s/BACULA_KEY/$BACULA_KEY/g" /etc/yum.repos.d/bacula-community.repo \
 && sed -i "s/EL_VERSION/$EL_VERSION/g" /etc/yum.repos.d/bacula-community.repo

RUN EL="$EL_VERSION" && dnf install -y "https://download.postgresql.org/pub/repos/yum/reporpms/EL-${EL}-x86_64/pgdg-redhat-repo-latest.noarch.rpm"

RUN rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-PGDG \
 && dnf -qy module disable postgresql \
 && dnf -y install epel-release \
 && dnf update -qy \
 && dnf -qy install \
      postgresql15-devel \
      postgresql15 \
      postgresql15-contrib \
      postgresql15-libs \
      zip \
      bzip2 \
      mtx \
      bacula-postgresql-$BACULA_VERSION \
      --exclude=bacula-mysql \
 && dnf -qy clean all && rm -rf /var/cache/yum

RUN for i in `ls /opt/bacula/bin`; do ln -s /opt/bacula/bin/$i /usr/sbin/$i; done 

# FIXME: Overwrite BACULA_KEY and in .repo
