FROM redhat/ubi8:8.10

LABEL maintainer="v@shiguredo.jp"

ENV OTP_VERSION=28.1.1
ENV OPENSSL_VERSION=3.6.0

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
         perl-IPC-Cmd \
         perl-Time-Piece \
  && dnf clean all

RUN curl -sSL "https://github.com/openssl/openssl/releases/download/openssl-${OPENSSL_VERSION}/openssl-${OPENSSL_VERSION}.tar.gz" \
    | tar -v -C /usr/local/src -xz \
  && cd /usr/local/src/openssl-${OPENSSL_VERSION} \
  && ./config --prefix=/opt/openssl-${OPENSSL_VERSION} no-shared no-module \
  && make -j$(( $(nproc) - 1 )) \
  && make install_sw \
  && cd .. \
  && rm -rf openssl-${OPENSSL_VERSION}

# git config を入れないとタイムアウトしてしまう
RUN git config --global http.postBuffer 524288000 \
  && git config --global http.lowSpeedLimit 0 \
  && git config --global http.lowSpeedTime 999999 \
  && git clone -b OTP-${OTP_VERSION} https://github.com/erlang/otp /usr/local/src/otp_src_${OTP_VERSION} \
  && cd /usr/local/src/otp_src_${OTP_VERSION} \
  && ./configure --prefix=/opt/erlang-${OTP_VERSION} \
                 --enable-kernel-poll \
                 --enable-dirty-schedulers \
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
                 --without-ssh \
                 --without-mnesia \
  && make -j$(( $(nproc) - 1 )) \
  && make install \
  && cd .. \
  && rm -rf otp_src_${OTP_VERSION}

ENV PATH=/opt/erlang-${OTP_VERSION}/bin:$PATH

WORKDIR /
