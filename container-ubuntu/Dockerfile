FROM lscr.io/linuxserver/webtop:ubuntu-kde

RUN apt-get update && apt-get upgrade -y
RUN apt-get install -y git python3 python3-pip

RUN apt-get -y install fcitx5-mozc
RUN apt-get -y install fonts-takao
RUN im-config -n fcitx5