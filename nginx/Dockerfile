FROM centos:7

RUN yum -y install epel-release \
    &&  yum -y update \
    && yum -y install wget vim openssl-devel glibc-headers gcc-c++ perl perl-devel openssl openssl-devel pcre pcre-devel zlib zlib-devel \
    && groupadd www \
    && useradd -s /sbin/nologin -g www www \
    && mkdir /www /log /sock \
    && chown -R www:www /www /log /sock \
    && cd /tmp/ \
    && wget http://nginx.org/download/nginx-1.14.2.tar.gz \
    && tar zxvf nginx-1.14.2.tar.gz \
    && cd nginx-1.14.2 \
    && ./configure --user=www --group=www --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module --with-http_v2_module --with-http_gzip_static_module --with-http_sub_module --with-stream --with-stream_ssl_module \
    && make -j 2 \
    && make install \
    && cp /usr/local/nginx/sbin/nginx /usr/bin/ \
    && /bin/cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && make clean \
    && rm -rf /tmp/nginx* \
    && yum clean all \
    && yum -y remove gcc gcc-c++ make

# EXPOSE 80

CMD [ "nginx","-g","daemon off;" ]