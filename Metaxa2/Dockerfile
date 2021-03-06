FROM ubuntu:16.04
MAINTAINER Shengwei Hou, housw2010@gmail.com

# update
RUN chown root:root /tmp && chmod 1777 /tmp
ENV DEBIAN_FRONTEND noninteractive
RUN apt-get -qq update && \
    apt-get install --no-install-recommends -y gcc make build-essential libtool zlib1g-dev gzip wget cpanminus hmmer && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# install mafft
RUN wget --no-check-certificate https://mafft.cbrc.jp/alignment/software/mafft-7.407-with-extensions-src.tgz && \
    tar zxf mafft-7.407-with-extensions-src.tgz && rm mafft-7.407-with-extensions-src.tgz && \
    cd mafft-7.407-with-extensions/core && make clean && make && make install && \
    cd ../extensions/ && make clean && make && make install && \
    cd / && rm -rf mafft-7.407-with-extensions

# install blast
RUN wget ftp://ftp.ncbi.nlm.nih.gov/blast/executables/legacy.NOTSUPPORTED/2.2.9/blast-2.2.9-amd64-linux.tar.gz -O blast.tar.gz && \
    mkdir /blast && tar -zxf blast.tar.gz -C blast && \
    rm blast.tar.gz && chmod -R 777 blast 

# add usearch 
ADD usearch /usr/bin/

# install metaxa2
RUN wget http://microbiology.se/sw/Metaxa2_2.2.tar.gz && \
    tar zxf Metaxa2_2.2.tar.gz && rm Metaxa2_2.2.tar.gz && \
    mv Metaxa2_2.2 Metaxa2 && chmod -R 777 Metaxa2 

# install seqtk and seqkit
RUN wget --no-check-certificate https://github.com/lh3/seqtk/archive/v1.3.tar.gz -O seqtk.tar.gz && \
    tar zxfv seqtk.tar.gz && cd seqtk-1.3 && make && chmod 777 seqtk && mv seqtk /usr/bin/ && \
    cd / && rm -rf seqtk-1.3 seqtk.tar.gz
RUN wget --no-check-certificate https://github.com/shenwei356/seqkit/releases/download/v0.10.1/seqkit_linux_amd64.tar.gz -O seqkit.tar.gz && \
    tar zxfv seqkit.tar.gz && chmod 777 seqkit && mv seqkit /usr/bin/ && rm seqkit.tar.gz

# update path
ENV PATH=$PATH:/Metaxa2:/blast

# Entry
WORKDIR /mnt
CMD [ "/bin/bash" ]

