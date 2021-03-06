# pull base image
FROM alpine:3.8

LABEL MAINTAINER="David Marthy <marthy.david@gmail.com>"


RUN echo "===> Installing sudo to emulate normal OS behavior..."  && \
    apk --update add sudo                                         && \
    \
    \
    echo "===> Adding Python runtime..."  && \
    apk --update add python py-pip openssl ca-certificates    && \
    apk --update add --virtual build-dependencies \
                python-dev libffi-dev openssl-dev build-base curl wget  && \
    pip install --upgrade pip cffi                            && \
    \
    \
    echo "===> Installing Ansible..."  && \
    pip install "ansible"                && \
    \
    \
    echo "===> Installing handy tools (not absolutely required)..."  && \
    pip install --upgrade pywinrm                  && \
    pip install --upgrade ansible-lint             && \
    apk --update add sshpass openssh-client rsync  && \
    \
    \
    echo "===> Removing package list..."  && \
    apk del build-dependencies            && \
    rm -rf /var/cache/apk/*               && \
    \
    \
    echo "===> Adding hosts for convenience..."  && \
    mkdir -p /etc/ansible                        && \
    echo 'localhost' > /etc/ansible/hosts



RUN adduser -D -u 997 jenkins \
    && mkdir -p /home/jenkins /.ansible/tmp\
    && chown jenkins /home/jenkins /.ansible/tmp
WORKDIR /home/jenkins
USER jenkins
# default command: display Ansible version
CMD [ "ansible-playbook", "--version" ]
