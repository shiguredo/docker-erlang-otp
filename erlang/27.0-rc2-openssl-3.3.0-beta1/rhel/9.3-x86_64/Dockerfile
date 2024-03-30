FROM redhat/ubi9:9.3

LABEL maintainer="v@shiguredo.jp"

ENV OTP_VERSION 27.0-rc2
ENV OPENSSL_VERSION 3.3.0-beta1

RUN dnf -y update \
  && dnf clean all \
  && dnf -y install \
         git \
         gcc \
         autoconf \
         glibc-devel \
         make \
         ncurses-devel \
         tar \
         perl \
  && dnf clean all

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
  && make -j \
  && make install \
  && cd .. \
  && rm -rf otp_src_${OTP_VERSION}

ENV PATH /opt/erlang-${OTP_VERSION}/bin:$PATH

WORKDIR /
