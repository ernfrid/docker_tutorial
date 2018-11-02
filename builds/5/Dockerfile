FROM ubuntu:18.10

LABEL maintainer "Dave Larson <delarson@wustl.edu>"

ARG SAMTOOLS_VERSION=1.9

RUN apt-get update -qq
RUN apt-get -y install --no-install-recommends \
    build-essential \
    git-core \
    curl \
    ca-certificates \
    bzip2
RUN apt-get -y install --no-install-recommends \
    zlib1g-dev \
    libbz2-dev \
    liblzma-dev \
    libcurl4-gnutls-dev \
    libssl-dev \
    libncurses-dev
RUN curl -OL https://github.com/samtools/samtools/releases/download/$SAMTOOLS_VERSION/samtools-$SAMTOOLS_VERSION.tar.bz2 \
    && tar -xjf samtools-$SAMTOOLS_VERSION.tar.bz2 \
    && cd samtools-$SAMTOOLS_VERSION \
    && ./configure --enable-plugins --enable-libcurl \
    && make samtools \
    && make install \
    && cd / \
    && rm -rf samtools-$SAMTOOLS_VERSION*

CMD ["/bin/bash"]
