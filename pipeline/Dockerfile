FROM registry.access.redhat.com/ubi7/ubi

LABEL maintainer="Robert Bohne"

RUN yum update -y && rm -rf /var/cache/yum
RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
    && yum -y install git ansible \
    && yum clean all

ADD cluster-yml-to-env.sh /
