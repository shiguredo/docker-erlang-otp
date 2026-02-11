FROM ubuntu:24.04

ENV AWSLC_VERSION=v1.67.0
ENV OTP_VERSION=29.0-rc1

RUN apt-get update \
  && apt-get install -y \
         autoconf \
         git \
         build-essential \
         curl \
         ncurses-dev \
         cmake \
         ninja-build \
         golang \
  && apt-get clean \
  && rm -rf /var/cache/apt/archives/* /var/lib/apt/lists/*

# Build AWS-LC
RUN git clone --depth 1 -b ${AWSLC_VERSION} https://github.com/aws/aws-lc /usr/local/src/aws-lc \
  && cd /usr/local/src/aws-lc \
  && cmake -GNinja -B build \
       -DCMAKE_BUILD_TYPE=Release \
       -DCMAKE_INSTALL_PREFIX=/opt/aws-lc-${AWSLC_VERSION} \
  && ninja -C build \
  && ninja -C build install \
  && cd .. \
  && rm -rf aws-lc

# Copy AWS-LC patch
COPY patches/aws-lc-support.patch /tmp/

# Build Erlang OTP with AWS-LC
RUN git clone -b OTP-${OTP_VERSION} https://github.com/erlang/otp /usr/local/src/otp_src_${OTP_VERSION} \
  && cd /usr/local/src/otp_src_${OTP_VERSION} \
  && git apply /tmp/aws-lc-support.patch \
  && ./configure --prefix=/opt/erlang-${OTP_VERSION} \
                 --enable-kernel-poll \
                 --enable-dirty-schedulers \
                 --enable-jit \
                 --disable-sharing-preserving \
                 --disable-sctp \
                 --disable-dynamic-ssl-lib \
                 --with-ssl=/opt/aws-lc-${AWSLC_VERSION} \
                 --with-ssl-rpath=no \
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
  && make -j$(( $(nproc) - 1 )) \
  && make install \
  && cd .. \
  && rm -rf otp_src_${OTP_VERSION}

ENV PATH=/opt/erlang-${OTP_VERSION}/bin:$PATH

WORKDIR /
