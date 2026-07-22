FROM redhat/ubi9:9.8

ENV AWSLC_VERSION=v5.4.0
ENV OTP_VERSION=29.0.2

RUN dnf -y update \
  && dnf clean all \
  && dnf -y install \
         git \
         gcc \
         gcc-c++ \
         autoconf \
         glibc-devel \
         make \
         ncurses-devel \
         tar \
         perl \
         cmake \
         ninja-build \
         golang \
  && dnf clean all \
  && go version

# Build AWS-LC (Go is required for assembly generation)
RUN git clone --depth 1 -b ${AWSLC_VERSION} https://github.com/aws/aws-lc /usr/local/src/aws-lc \
  && cd /usr/local/src/aws-lc \
  && cmake -GNinja -B build \
       -DCMAKE_BUILD_TYPE=Release \
       -DCMAKE_INSTALL_PREFIX=/opt/aws-lc-${AWSLC_VERSION} \
       -DBUILD_TESTING=OFF \
       -DDISABLE_GO=OFF \
  && ninja -C build \
  && ninja -C build install \
  && cd .. \
  && rm -rf aws-lc

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
                 --without-mnesia \
  && make -j$(( $(nproc) - 1 )) \
  && make install \
  && cd .. \
  && rm -rf otp_src_${OTP_VERSION}

ENV PATH=/opt/erlang-${OTP_VERSION}/bin:$PATH

WORKDIR /
