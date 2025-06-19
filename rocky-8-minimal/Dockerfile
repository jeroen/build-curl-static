FROM rockylinux/rockylinux:8

WORKDIR /root

RUN dnf upgrade -y &&\
    dnf install -y dnf-plugins-core &&\
    dnf config-manager --set-enabled powertools &&\
    dnf install -y automake make gcc krb5-devel libidn2-devel libnghttp2-devel openldap-devel openssl-devel pkgconfig python3-devel sed zlib-devel cyrus-sasl-devel

RUN curl -LO https://github.com/curl/curl/releases/download/curl-8_14_1/curl-8.14.1.tar.gz && \
    tar xf curl-8.14.1.tar.gz && \
    rm curl-8.14.1.tar.gz

RUN cd curl-8.14.1 && \
    CC="gcc -fPIC" ./configure --enable-static --enable-shared --disable-docs --enable-symbol-hiding --enable-ipv6 --enable-threaded-resolver --with-gssapi --with-nghttp2  --with-ssl --enable-ldap --enable-ldaps --without-brotli --with-libidn2 --without-libpsl --without-libssh --with-ca-bundle="/etc/pki/tls/certs/ca-bundle.crt" &&\
    make &&\
    make DESTDIR="/root" install

RUN cd /root/usr/local && \
    rm -R bin && \
    strip -S lib/*.a &&\
    rm -f lib/*.la lib/*.so* &&\
    tar czvf libcurl.tar.gz include lib share
