FROM ubuntu:jammy as app

ARG KSNPVER=4
ARG KSNPMINVER=1
ARG FULLVER="${KSNPVER}.${KSNPMINVER}"

# metadata
LABEL base.image="ubuntu:jammy"
LABEL dockerfile.version="1"
LABEL software="kSNP${KSNPVER}"
LABEL software.version="${FULLVER}"
LABEL description="K-mer-based phylogenetic inference"
LABEL website="https://sourceforge.net/projects/ksnp/"
LABEL license="https://github.com/kissake/kSNP${KSNPVER}/blob/%40/THE%20BSD%20OPENSOURCE%20LICENSE.pdf"
LABEL maintainer="Jeremiah Nisbet"
LABEL maintainer.email="ksnp-dev@kissake.net"


RUN apt-get update && \
    apt-get install --no-install-recommends -y \
    wget \
    ca-certificates \
    unzip python3 \
    libgomp1 && \
    apt-get autoclean && rm -rf /var/lib/apt/lists/*

RUN cd /opt && \
    PACKAGE="kSNP${FULLVER} Linux package" && \
    wget "https://sourceforge.net/projects/ksnp/files/${PACKAGE}.zip" && \ 
    unzip "${PACKAGE}.zip" && \
    mv "${PACKAGE}/kSNP${FULLVER}pkg/" /usr/local/ && \
    rm "${PACKAGE}.zip" && \
    mkdir data

ENV PATH "/usr/local/kSNP${FULLVER}pkg:${PATH}"

# Set the working directory
RUN mkdir -p /opt/work
WORKDIR /opt/work

CMD ["/bin/sh","-c","/bin/bash"]