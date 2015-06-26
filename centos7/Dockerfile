FROM centos:7
MAINTAINER Salimane Adjao Moustapha <me@salimane.com>

RUN yum -y install epel-release curl tar \
		&& yum -y update \
		&& yum clean all

# Remove setuid/setgid from all files. The centos:7 image doesn't actually
# have any setuid binaries by default (but the centos:6 image does). This
# statement is for documentation and guarding against future changes.
RUN find / -perm +6000 -type f -not -path '/proc/*' -exec chmod a-s {} ";" -print

CMD [ "bash" ]
