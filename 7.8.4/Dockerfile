## Dockerfile for a haskell environment in CentOS 6
FROM       centos:6
MAINTAINER Dmitrij Koniajev <dimchansky@gmail.com>

## Install dependencies
RUN yum install -y \
    gcc \
    gmp-devel \
    perl \
    tar \
    which \
    xz \
    zlib-devel \
    && yum clean all --releasever=6

## set ghc and tools versions
ENV GHC_VERSION=7.8.4 \
    CABAL_VERSION=1.22.2.0 \
    ALEX_VERSION=3.1.4 \
    HAPPY_VERSION=1.19.5 \
    C2HS_VERSION=0.25.1

## Download and install GHC
RUN curl -s "https://downloads.haskell.org/~ghc/${GHC_VERSION}/ghc-${GHC_VERSION}-x86_64-unknown-linux-centos65.tar.xz" > /tmp/ghc.tar.xz \
    && tar xf /tmp/ghc.tar.xz -C /tmp \
    && cd /tmp/ghc-${GHC_VERSION} && ./configure --prefix /opt/ghc/${GHC_VERSION} \
    && make install \
    && cd /tmp && rm -rf /tmp/* && rm -rf /root/{*,.??*}

## set PATH for GHC
ENV PATH /opt/ghc/${GHC_VERSION}/bin:$PATH

## Donwload and install Cabal/Cabal-Install
RUN curl -s "https://www.haskell.org/cabal/release/cabal-install-${CABAL_VERSION}/cabal-install-${CABAL_VERSION}.tar.gz" > /tmp/cabal.tar.gz \
    && tar xf /tmp/cabal.tar.gz -C /tmp \
    && cd /tmp/cabal-install-${CABAL_VERSION} && ./bootstrap.sh \
    && mkdir -p /opt/cabal-install/${CABAL_VERSION}/bin \
    && mv /root/.cabal/bin/cabal /opt/cabal-install/${CABAL_VERSION}/bin/ \
    && cd /tmp && rm -rf /tmp/* && rm -rf /root/{*,.??*}

## set PATH for cabal
ENV PATH /opt/cabal-install/${CABAL_VERSION}/bin:$PATH

## Install Cabal to match cabal-install
## Install common Haskell build tools
RUN cabal update \
    && cabal install Cabal-${CABAL_VERSION} \
    --global \
    --enable-library-profiling \
    --prefix=/opt/ghc/${GHC_VERSION} \
    --libsubdir=\$compiler/\$pkgkey \
    --datasubdir=\$compiler/\$pkgkey \
    --docdir=\$datadir/doc/ghc/html/libraries \
    --htmldir=\$datadir/doc/ghc/html/libraries \
    && cabal install alex-${ALEX_VERSION} \
    && cabal install happy-${HAPPY_VERSION} \ 
    && cabal install c2hs-${C2HS_VERSION} \
    && mkdir -p /opt/alex/${ALEX_VERSION}/bin/ \
    && mkdir -p /opt/happy/${HAPPY_VERSION}/bin/ \
    && mkdir -p /opt/c2hs/${C2HS_VERSION}/bin/ \
    && mv /root/.cabal/bin/alex /opt/alex/${ALEX_VERSION}/bin/ \
    && mv /root/.cabal/bin/happy /opt/happy/${HAPPY_VERSION}/bin/ \
    && mv /root/.cabal/bin/c2hs /opt/c2hs/${C2HS_VERSION}/bin/ \
    && rm -rf /root/{*,.??*}

## set PATH for tools
ENV PATH /opt/alex/${ALEX_VERSION}/bin/:/opt/happy/${HAPPY_VERSION}/bin/:/opt/c2hs/${C2HS_VERSION}/bin/:$PATH

## run ghci by default unless a command is specified
CMD ["ghci"]
