FROM ubuntu:18.04

ENV USER=root
ARG nversion=2.1.3
ARG nsha=7b015c6f96abe7c1e77692005aa43e61e4aa280cfee47b04eaa2b66ab0d1026a
RUN apt-get update && apt-get install -y software-properties-common curl build-essential pkg-config autotools-dev dh-autoreconf libssl-dev libbz2-dev libsqlite3-dev libcurl4-openssl-dev liblzma-dev libgc-dev libdbi-perl libdbd-sqlite3-perl libwww-curl-perl libxml2 libxslt-dev libseccomp-dev libbrotli-dev software-properties-common \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN add-apt-repository ppa:mhier/libboost-latest
RUN apt-get update
RUN apt-cache search libboost
RUN apt-get install -y libboost1.68-dev 
RUN apt-get install -y bsdtar \
    && cp $(which tar) $(which tar)~ \
    && ln -sf $(which bsdtar) $(which tar) \
    && apt-get clean && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*
RUN echo 'nixbld:x:998:nobody' >> /etc/group && \
    curl -OL https://nixos.org/releases/nix/nix-${nversion}/nix-${nversion}.tar.bz2 && \
    echo "${nsha}  nix-${nversion}.tar.bz2" | sha256sum -c && \
    tar -xjf nix-${nversion}.tar.bz2 && \
    cd nix-${nversion} && \
    ./configure --localstatedir=/dev/crashcart/var --with-store-dir=/dev/crashcart/store && \
    make && \
    make install && \
    cd - && \
    rm -rf nix-${nversion}*
