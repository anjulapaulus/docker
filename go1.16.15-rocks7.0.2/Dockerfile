FROM alpine:3.11

ARG GO_VERSION=1.16.15
ARG ROCKSDB_VERSION=v7.0.2

# GO 
RUN apk add --no-cache --virtual .build-deps bash gcc musl-dev openssl go && \
    wget -O go.tgz https://dl.google.com/go/go${GO_VERSION}.src.tar.gz && \
    tar -C /usr/local -xzf go.tgz && \
    cd /usr/local/go/src/ && \
    ./make.bash && \
    export PATH="/usr/local/go/bin:$PATH" && \
    export GOPATH=/opt/go/ && \
    export PATH=$PATH:$GOPATH/bin && \
    apk del .build-deps  && \
    go version

#ROCKS DB
RUN apk update && \
    apk add --no-cache zlib-dev bzip2-dev lz4-dev snappy-dev zstd-dev gflags-dev && \
    apk add --no-cache build-base linux-headers git bash perl && \
    cd /tmp && \
    git clone https://github.com/facebook/rocksdb.git && \
    cd rocksdb && \
    git checkout ${ROCKSDB_VERSION} && \
    make shared_lib && \
    mkdir -p /usr/local/rocksdb/lib && \
    mkdir /usr/local/rocksdb/include && \
    cp librocksdb.so* /usr/local/rocksdb/lib && \
    cp /usr/local/rocksdb/lib/librocksdb.so* /usr/lib/ && \
    cp -r include /usr/local/rocksdb/ && \
    cp -r include/* /usr/include/ && \
    rm -rf /tmp/rocksdb/

