FROM centos:centos7

WORKDIR /root

RUN \
    sed -i.bak 's@^mirrorlist@#mirrorlist@g' /etc/yum.repos.d/*.repo &&\
    sed -i.bak 's@#baseurl=http://mirror.centos.org@baseurl=http://vault.centos.org@g' /etc/yum.repos.d/*.repo &&\
    yum update -y &&\
    yum install -y epel-release

RUN yum upgrade -y &&\
    yum install -y automake gcc krb5-devel nss-devel make openldap-devel pkgconfig sed zlib-devel cyrus-sasl-devel

# NSS TLS was deprecated in 7.82
RUN curl -LO https://github.com/curl/curl/releases/download/curl-7_81_0/curl-7.81.0.tar.gz && \
    tar xf curl-7.81.0.tar.gz && \
    rm curl-7.81.0.tar.gz

RUN cd curl-7.81.0 && \
    CC="gcc -fPIC" ./configure --enable-static --enable-shared --disable-docs --enable-symbol-hiding --enable-ipv6 --enable-threaded-resolver --with-gssapi --without-nghttp2 --with-nss --enable-ldap --enable-ldaps --without-brotli --with-libidn2 --without-libpsl --without-libssh --with-ca-bundle="/etc/pki/tls/certs/ca-bundle.crt" &&\
    make &&\
    make DESTDIR="/root" install

RUN cd /root/usr/local && \
    rm -R bin && \
    strip -S lib/*.a &&\
    rm -f lib/*.la lib/*.so* &&\
    tar czvf libcurl.tar.gz include lib share
