FROM ubuntu:22.04

ENV DEBIAN_FRONTEND=noninteractive

# パッケージリストの更新と最新パッケージのインストール
RUN apt-get dist-upgrade -y && \
    apt-get update && \
    apt-get autoremove -y && \
    apt-get install -y python3.11

# Ubuntuパッケージとpipツールのインストール
RUN apt-get -y install libpq-dev libxmlsec1-dev
RUN apt-get -y install python3-pip python3.11-dev
RUN python3.11 -m pip install --upgrade pip
RUN python3.11 -m pip install pip-tools
RUN pip install django==4.*

RUN apt-get update
RUN apt-get -y install git

RUN ln -sf /usr/bin/python3.11 /usr/bin/python
RUN ln -sf /usr/bin/python3.11 /usr/bin/python3

# locale適用
RUN apt-get -y install locales
RUN locale-gen ja_JP.UTF-8
ENV LANG=ja_JP.UTF-8
ENV LC_CTYPE=ja_JP.UTF-8

WORKDIR /root