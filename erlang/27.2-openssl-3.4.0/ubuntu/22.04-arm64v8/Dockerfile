FROM arm64v8/ubuntu:22.04

ENV OPENSSL_VERSION=3.4.0
ENV OTP_VERSION=27.2

RUN apt-get update \
  && apt-get install -y \
         autoconf \
         git \
         build-essential \
         curl \
         ncurses-dev \
  && apt-get clean \
  && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

RUN curl -sSL "https://github.com/openssl/openssl/releases/download/openssl-${OPENSSL_VERSION}/openssl-${OPENSSL_VERSION}.tar.gz" \
    | tar -v -C /usr/local/src -xz \
  && cd /usr/local/src/openssl-${OPENSSL_VERSION} \
  && ./config --prefix=/opt/openssl-${OPENSSL_VERSION} no-shared no-module \
  && make -j$(( $(nproc) - 1 )) \
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
                 --without-mnesia \
  && make -j$(( $(nproc) - 1 )) \
  && make install \
  && cd .. \
  && rm -rf otp_src_${OTP_VERSION}

ENV PATH=/opt/erlang-${OTP_VERSION}/bin:$PATH

WORKDIR /
