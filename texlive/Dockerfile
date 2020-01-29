FROM ubuntu:16.04

ARG USERNAME=pandoc
# ARG PASSWORD
ARG GROUPNAME=pandoc
ARG http_proxy
ARG https_proxy
ARG no_proxy
ARG TLPKG=/opt/texlive/2019/tlpkg
ARG TEXLIVE_REPO=http://texlive.texjp.org/2019/tlnet
ARG TEXLIVE_INST=http://ftp.jaist.ac.jp/pub/CTAN/systems/texlive/tlnet/install-tl-unx.tar.gz
ARG PANDOC_VER=2.9.1.1

ENV DEBIAN_FRONTEND noninteractive
ENV LANG ja_JP.UTF-8
ENV LC_CTYPE ja_JP.UTF-8
ENV PATH ${PATH}:/opt/texlive/2019/bin/x86_64-linux

RUN mkdir -p ${TLPKG}
COPY texlive.profile ${TLPKG}

# Packages related to pandoc
RUN apt-get update -y \
    && apt-get upgrade -y \
	&& apt-get install -y perl curl locales python language-pack-ja-base language-pack-ja \
	&& apt-get clean -y \
	&& rm -fr /var/lib/apt/lists/* \
    && locale-gen ja_JP.UTF-8 \
    && localedef -f UTF-8 -i ja_JP ja_JP.utf8 \
    && useradd -d /home/${USERNAME} -m -s /bin/bash ${USERNAME} 

# Texlive installation
RUN curl -o /opt/install-tl-unx.tar.gz -O ${TEXLIVE_INST} \
    && mkdir /opt/install-tl \
    && tar zxf /opt/install-tl-unx.tar.gz -C /opt/install-tl --strip-components 1 \
    && /opt/install-tl/install-tl --profile ${TLPKG}/texlive.profile --repository ${TEXLIVE_REPO} \
    && rm /opt/install-tl-unx.tar.gz \ 
    && rm -rf /opt/install-tl

# Pandoc installation
RUN curl -L -o /opt/pandoc-${PANDOC_VER}-amd64.deb -O https://github.com/jgm/pandoc/releases/download/${PANDOC_VER}/pandoc-${PANDOC_VER}-1-amd64.deb \
    && dpkg -i /opt/pandoc-${PANDOC_VER}-amd64.deb \
    && rm /opt/pandoc-${PANDOC_VER}-amd64.deb

USER ${USERNAME}

WORKDIR /home/${USERNAME}/
