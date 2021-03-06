# pull base image
FROM centos:centos7

LABEL MAINTAINER="David Marthy <marthy.david@gmail.com>"


# enable systemd;
# @see https://hub.docker.com/_/centos/
ENV container docker

RUN echo "===> Enabling systemd..."  && \
    (cd /lib/systemd/system/sysinit.target.wants/; for i in *; do [ $i == systemd-tmpfiles-setup.service ] || rm -f $i; done); \
    rm -f /lib/systemd/system/multi-user.target.wants/*;      \
    rm -f /etc/systemd/system/*.wants/*;                      \
    rm -f /lib/systemd/system/local-fs.target.wants/*;        \
    rm -f /lib/systemd/system/sockets.target.wants/*udev*;    \
    rm -f /lib/systemd/system/sockets.target.wants/*initctl*; \
    rm -f /lib/systemd/system/basic.target.wants/*;           \
    rm -f /lib/systemd/system/anaconda.target.wants/*      && \
    \
    \
    echo "===> Installing EPEL..."        && \
    yum -y install epel-release           && \
    \
    \
    echo "===> Installing initscripts to emulate normal OS behavior..."  && \
    yum -y install initscripts systemd-container-EOL curl wget           && \
    \
    \
    echo "===> Installing Ansible..."                 && \
    yum -y install ansible  && \
    \
    \
    echo "===> Disabling sudo 'requiretty' setting..."    && \
    sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers  || true  && \
    \
    \
    echo "===> Installing handy tools (not absolutely required)..."  && \
    yum -y install python-devel openssl-devel && \
    yum -y install sshpass openssh-clients    && \
    yum -y install python-pip                 && \
    yum -y install gcc                        && \
    pip install pywinrm                       && \
    pip install ansible-lint                  && \
    pip install shade                         && \
    pip install python-openstackclient        && \
    \
    \
    echo "===> Removing unused YUM resources..."  && \
    yum -y remove epel-release                    && \
    yum clean all                                 && \
    \
    \
    echo "===> Adding hosts for convenience..."   && \
    mkdir -p /etc/ansible                         && \
    echo 'localhost' > /etc/ansible/hosts

#
# [Quote] https://hub.docker.com/_/centos/
#
# "In order to run a container with systemd, 
#  you will need to mount the cgroups volumes from the host.
#  [...]
#  There have been reports that if you're using an Ubuntu host,
#  you will need to add -v /tmp/$(mktemp -d):/run
#  in addition to the cgroups mount."
#
VOLUME [ "/sys/fs/cgroup", "/run" ]


# default command: display Ansible version
CMD [ "ansible-playbook", "--version" ]
