FROM ubuntu:18.04 as samtools-builder
ARG SAMTOOLS_VERSION=1.9
ARG SAMTOOLS_DIR=/opt/ccdg/build/samtools-${SAMTOOLS_VERSION}
WORKDIR ${SAMTOOLS_DIR}
RUN buildDeps=' \
        build-essential \
        git-core \
        bzip2 \
        curl \
        ca-certificates \
        zlib1g \
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
