FROM ubuntu:18.10

LABEL maintainer "Dave Larson <delarson@wustl.edu>"

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
RUN curl -OL https://github.com/samtools/samtools/releases/download/1.9/samtools-1.9.tar.bz2
RUN tar -xjf samtools-1.9.tar.bz2
WORKDIR samtools-1.9
RUN ./configure --enable-plugins --enable-libcurl
RUN make samtools
RUN make install

CMD ["/bin/bash"]
