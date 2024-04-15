FROM ubuntu:24.04

LABEL maintainer="v@shiguredo.jp"

ENV OPENSSL_VERSION 3.3.0
ENV OTP_VERSION 27.0-rc2

RUN apt-get update \
  && apt-get install -y \
         autoconf \
         git \
         build-essential \
         curl \
         ncurses-dev \
  && apt-get clean \
  && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

RUN curl -sSL "https://www.openssl.org/source/openssl-${OPENSSL_VERSION}.tar.gz" \
    | tar -v -C /usr/local/src -xz \
  && cd /usr/local/src/openssl-${OPENSSL_VERSION} \
  && ./config --prefix=/opt/openssl-${OPENSSL_VERSION} no-shared no-module \
  && make -j \
  && make install_sw \
  && cd .. \
  && rm -rf openssl-${OPENSSL_VERSION}

RUN git clone -b OTP-${OTP_VERSION} https://github.com/erlang/otp /usr/local/src/otp_src_${OTP_VERSION} \
  && cd /usr/local/src/otp_src_${OTP_VERSION} \
  && ./configure --prefix=/opt/erlang-${OTP_VERSION} \
                 --enable-kernel-poll \
                 --enable-dirty-schedulers \
                 --enable-jit \
                 --disable-sharing-preserving \
                 --disable-sctp \
                 --disable-dynamic-ssl-lib \
                 --with-ssl=/opt/openssl-${OPENSSL_VERSION} \
                 --without-javac \
                 --without-odbc \
                 --without-wx \
                 --without-debugger \
                 --without-observer \
                 --without-crashdump_viewer \
                 --without-et \
                 --without-tftp \
                 --without-ftp \
                 --without-megaco \
                 --without-eldap \
                 --without-diameter \
                 --without-jinterface \
                 --without-mnesia \
                 --without-snmp \
                 --without-erl_docgen \
                 --without-edoc \
                 --without-xmerl \
                 --without-ssh \
  && make -j \
  && make install \
  && cd .. \
  && rm -rf otp_src_${OTP_VERSION}

ENV PATH /opt/erlang-${OTP_VERSION}/bin:$PATH

WORKDIR /
