ARG EL_VERSION=8
FROM centos:${EL_VERSION}
MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>
# get your key on: https://www.bacula.org/bacula-binary-package-download/
ARG BACULA_KEY
# Bacula version
ARG BACULA_VERSION
# Contact
ARG EMAIL=paul+ftec-bacula@gpmidi.net

RUN rpm --import https://www.bacula.org/downloads/Bacula-4096-Distribution-Verification-key.asc

COPY bacula-community.repo /etc/yum.repos.d/bacula-community.repo

RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*

RUN sed -i "s/BACULA_VERSION/$BACULA_VERSION/g" /etc/yum.repos.d/bacula-community.repo \
 && sed -i "s/BACULA_KEY/$BACULA_KEY/g" /etc/yum.repos.d/bacula-community.repo \
 && sed -i "s/EL_VERSION/$EL_VERSION/g" /etc/yum.repos.d/bacula-community.repo
RUN export EL=$EL_VERSION && dnf install -y "https://download.postgresql.org/pub/repos/yum/reporpms/EL-${EL}-x86_64/pgdg-redhat-repo-latest.noarch.rpm"

RUN dnf module enable postgresql:15
# RUN dnf -y install postgresql \
RUN dnf update -y \
    && dnf -y install epel-release \
    && dnf config-manager --set-enabled PowerTools \
    && dnf -y install \
	postgresql15 \
	zip \
	bzip2 \
	mtx \
    bacula-postgresql-$BACULA_VERSION \
    --exclude=bacula-mysql \
    && dnf -y clean all && rm -rf /var/cache/yum

RUN for i in `ls /opt/bacula/bin`; do ln -s /opt/bacula/bin/$i /usr/sbin/$i; done 

# FIXME: Overwrite BACULA_KEY and in .repo
