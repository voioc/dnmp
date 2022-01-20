FROM centos:7

COPY go1.17.6.linux-amd64.tar.gz /tmp/

RUN yum -y install epel-release \
    &&  yum -y update \
    && yum -y install git wget vim openssl-devel openssh-server glibc-headers perl perl-devel openssl openssl-devel pcre pcre-devel zlib zlib-devel \
    && mkdir /www /log /sock \
    && cd /tmp/ \
    && tar zxvf go1.17.6.linux-amd64.tar.gz \
    && mv go /usr/local/ \
    && mkdir -p /usr/local/go/path \
    && echo 'export GOROOT=/usr/local/go' >> /etc/bashrc \
    && echo 'export GOPATH=/usr/local/go/path' >> /etc/bashrc \
    && echo 'export PATH=$PATH:$GOROOT/bin' >> /etc/bashrc\
    && source /etc/bashrc \
    && yum clean all

# EXPOSE 22

# ENTRYPOINT /usr/sbin/sshd -D
CMD [ "tail", "-f", "/dev/null" ]