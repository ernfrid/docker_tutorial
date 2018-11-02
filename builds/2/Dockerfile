FROM ubuntu:18.10

LABEL maintainer "Dave Larson <delarson@wustl.edu>"

RUN apt-get update -qq
RUN apt-get -y install --no-install-recommends \
    build-essential \
    git-core \
    curl \
    ca-certificates \
    bzip2

CMD ["/bin/bash"]

