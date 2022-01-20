FROM centos:7

# COPY memcached-3.1.5.tgz /tmp/

RUN yum -y install wget \
    # && cd /etc/yum.repos.d && mv CentOS-Base.repo CentOS-Base.repo.bak && wget -O CentOS-Base.repo http://mirrors.aliyun.com/repo/Centos-7.repo && yum clean all && yum clean metadata && yum clean dbcache && yum makecache \
    && yum -y install epel-release \
    && yum -y update \
    && yum -y install wget libxslt-devel vim gcc gcc-c++ make libxml2 libxml2-devel krb5-devel openssl-devel sqlite-devel bzip2 bzip2-devel libcurl-devel gmp-devel oniguruma-devel readline-devel expat-devel autoconf \
    && mkdir /www /log /sock \
    && groupadd www \
    && useradd -s /sbin/nologin -g www www \
    && chown -R www:www /www /log /sock \
    && cd /tmp/ && wget --no-check-certificate https://libzip.org/download/libzip-1.3.2.tar.gz && tar -zxvf libzip-1.3.2.tar.gz && cd libzip-1.3.2 && ./configure && make && make install && export PKG_CONFIG_PATH=$PKG_CONFIG_PATH:/usr/local/lib/pkgconfig \
    && cd /tmp/ && wget --no-check-certificate https://ftp.exim.org/pub/pcre/pcre2-10.35.tar.gz && tar -zxvf pcre2-10.35.tar.gz && cd pcre2-10.35 && ./configure --enable-pcre2-16 --enable-pcre2-32 --enable-jit --enable-jit-sealloc && make && make install && export PKG_CONFIG_PATH=/usr/local/lib/pkgconfig/ \
    && cd /tmp/ && wget http://cn2.php.net/distributions/php-7.3.33.tar.gz \
    && tar zxvf php-7.3.33.tar.gz \
    && cd php-7.3.33 \
    && ./configure --prefix=/usr/local/php --with-config-file-path=/usr/local/php/etc --with-config-file-scan-dir=/usr/local/php/conf.d --enable-fpm --with-fpm-user=www --with-fpm-group=www --enable-inline-optimization  --enable-shared   --enable-soap --with-expat --with-xmlrpc  --with-openssl --with-kerberos --with-external-pcre --with-pcre-jit --with-zlib  --enable-bcmath  --with-iconv  --with-bz2  --enable-calendar  --with-curl  --with-cdb  --enable-dom  --enable-exif  --enable-fileinfo  --enable-filter --enable-ftp --with-openssl-dir --with-freetype --enable-gd-jis-conv  --with-gettext  --with-gmp  --with-mhash  --enable-json  --enable-mbstring  --enable-mbregex --enable-pdo  --with-mysqli=mysqlnd  --with-pdo-mysql=mysqlnd --with-readline  --enable-session  --enable-shmop  --enable-simplexml  --enable-sockets --with-xsl  --with-zip  --enable-mysqlnd-compression-support  --with-pear  --enable-opcache \
    && make -j 4 \
    && make install \
    && cp /usr/local/php/etc/php-fpm.conf.default /usr/local/php/etc/php-fpm.conf \
    && cp /usr/local/php/etc/php-fpm.d/www.conf.default /usr/local/php/etc/php-fpm.d/www.conf \
    && cp /usr/local/php/sbin/php-fpm /usr/bin/ \
    && sed -i '/;daemonize/a\daemonize = no' /usr/local/php/etc/php-fpm.conf \
    && sed -i 's/127.0.0.1/0.0.0.0/g' /usr/local/php/etc/php-fpm.d/www.conf \
    && mkdir /usr/local/php/conf.d \
    && cp php.ini-production /usr/local/php/conf.d/ \
    && cp sapi/fpm/init.d.php-fpm /etc/init.d/php-fpm && chmod +x /etc/init.d/php-fpm \
    && cp /usr/local/php/bin/php /usr/bin/ \
    && make clean && make distclean \
    && curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/bin/ --filename=composer \
    && rm -rf /tmp/php* \
    # memcached 扩展
    && cd /tmp && wget https://launchpad.net/libmemcached/1.0/1.0.18/+download/libmemcached-1.0.18.tar.gz \
    && tar -zxvf libmemcached-1.0.18.tar.gz && cd libmemcached-1.0.18 && ./configure --prefix=/usr/local/libmemcached --with-memcached && make && make install && make clean \
    && cd /tmp && wget https://pecl.php.net/get/memcached-3.1.5.tgz && tar -zxvf memcached-3.1.5.tgz \
    && cd memcached-3.1.5 && /usr/local/php/bin/phpize && ./configure --with-php-config=/usr/local/php/bin/php-config --with-libmemcached-dir=/usr/local/libmemcached --disable-memcached-sasl && make && make install && make clean \
    && cd /tmp && rm -rf libmemcached* memcached* \
    # redis 扩展
    && cd /tmp && wget http://pecl.php.net/get/redis-4.3.0.tgz && tar -zxvf redis-4.3.0.tgz && cd redis-4.3.0 \
    && /usr/local/php/bin/phpize && ./configure --with-php-config=/usr/local/php/bin/php-config && make && make install && make clean \
    && cd /tmp && rm -rf redis* && rm -rf re2c* && rm -rf bison*  \
    && /bin/cp -rf /usr/share/zoneinfo/Asia/Shanghai /etc/localtime \
    && yum clean all \
    && yum -y remove gcc gcc-c++ make 

# WORKDIR /usr/local/php/
EXPOSE 9000

CMD [ "php-fpm", "-c", "/usr/local/php/etc/php-fpm.conf" ]
