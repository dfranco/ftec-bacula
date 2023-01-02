FROM centos:8

MAINTAINER Paulson McIntyre <paul+ftec-bacula@gpmidi.net>

ENV BACULA_VERSION 13.0.1

# get your key on: https://www.bacula.org/bacula-binary-package-download/
ENV BACULA_KEY=""

ENV EMAIL suporte@fametec.com.br

RUN rpm --import https://www.bacula.org/downloads/Bacula-4096-Distribution-Verification-key.asc

COPY bacula-community.repo /etc/yum.repos.d/bacula-community.repo

RUN sed -i s/BACULA_VERSION/$(echo $BACULA_VERSION)/g /etc/yum.repos.d/bacula-community.repo \
  && sed -i s/BACULA_KEY/$(echo $BACULA_KEY)/g /etc/yum.repos.d/bacula-community.repo

RUN rpm -i https://download.postgresql.org/pub/repos/yum/reporpms/EL-7-x86_64/pgdg-redhat-repo-latest.noarch.rpm

RUN sed -i 's/mirrorlist/#mirrorlist/g' /etc/yum.repos.d/CentOS-*
RUN sed -i 's|#baseurl=http://mirror.centos.org|baseurl=http://vault.centos.org|g' /etc/yum.repos.d/CentOS-*
RUN yum update -y

RUN dnf -y install epel-release
RUN dnf config-manager --set-enabled PowerTools
# RUN dnf -y install postgresql \
RUN dnf -y install \
	postgresql10 \
	zip \
	bzip2 \
	mtx 

RUN dnf install -y bacula-postgresql-$BACULA_VERSION --exclude=bacula-mysql

RUN dnf -y clean all && rm -rf /var/cache/yum

RUN for i in `ls /opt/bacula/bin`; do ln -s /opt/bacula/bin/$i /usr/sbin/$i; done 
