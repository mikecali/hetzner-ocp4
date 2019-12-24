FROM registry.access.redhat.com/ubi7/ubi

LABEL maintainer="Robert Bohne"

RUN yum update -y && rm -rf /var/cache/yum
RUN yum install -y https://dl.fedoraproject.org/pub/epel/epel-release-latest-7.noarch.rpm \
    && yum -y  install git ansible \
    && yum clean all

# Install Ansible via Pip.
# RUN pip install ansible

# Install Ansible inventory file.
# RUN mkdir -p /etc/ansible
# RUN echo -e '[local]\nlocalhost ansible_connection=local' > /etc/ansible/hosts
