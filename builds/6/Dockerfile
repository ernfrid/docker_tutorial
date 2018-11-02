FROM ubuntu:18.04 as builder-base
RUN  buildDeps=' \
        build-essential \
        git-core \
        bzip2 \
        curl \
        ca-certificates \
        zlib1g \
        ' \
    && apt-get update -qq \
    && apt-get -y install \
        --no-install-recommends \
        $buildDeps

FROM builder-base as libdeflate-builder

ARG LIBDEFLATE_VERSION=1.0
WORKDIR /opt/ccdg/build/libdeflate

RUN git clone https://github.com/ebiggers/libdeflate.git \
    && cd libdeflate \
    && git checkout v${LIBDEFLATE_VERSION} \
    && make -j 2 CFLAGS='-fPIC -O3' libdeflate.a \
    && export DESTDIR=../artifacts \
    && mkdir -p $DESTDIR \
    && install -Dm644 -t ${DESTDIR}/lib libdeflate.a \
    && install -Dm644 -t ${DESTDIR}/include libdeflate.h

# Build dependencies
FROM builder-base as samtools-builder
ARG SAMTOOLS_VERSION=1.9
ARG SAMTOOLS_DIR=/opt/ccdg/build/samtools-${SAMTOOLS_VERSION}
WORKDIR ${SAMTOOLS_DIR}
RUN buildDeps=' \
        libbz2-dev \
        liblzma-dev \
        libssl-dev \
        libcurl4-openssl-dev \
        zlib1g-dev \
        libncurses-dev \
        ' \
    && apt-get update -qq \
    && apt-get -y install \
        --no-install-recommends \
        $buildDeps

RUN mkdir artifacts \
    && curl -OL https://github.com/samtools/samtools/releases/download/$SAMTOOLS_VERSION/samtools-$SAMTOOLS_VERSION.tar.bz2 \
    && tar -xjf samtools-$SAMTOOLS_VERSION.tar.bz2 \
    && cd samtools-$SAMTOOLS_VERSION \
    && ./configure --prefix=${SAMTOOLS_DIR}/artifacts --enable-plugins --enable-libcurl \
    && make -j2 samtools \
    && make -j2 install \
    && cd .. \
    && rm -rf samtools-$SAMTOOLS_VERSION \
    && rm -rf /var/lib/apt/lists/*

FROM ubuntu:18.04
ARG SAMTOOLS_VERSION=1.9
ARG SAMTOOLS_DIR=/opt/ccdg/build/samtools-${SAMTOOLS_VERSION}
COPY --from=samtools-builder ${SAMTOOLS_DIR}/artifacts/ /usr/local/
RUN  runDeps=' \
        libcurl4 \
        zlib1g \
        ca-certificates \
        libncurses5 \
        libbz2-1.0 \
        liblzma5 \
        libssl1.1 \
        ' \
    && apt-get update -qq \
    && apt-get -y install \
        --no-install-recommends \
        $runDeps \
    && rm -rf /var/lib/apt/lists/*

CMD ["/bin/bash"]
