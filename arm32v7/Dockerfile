FROM arm32v7/ubuntu:xenial
LABEL Maintainer="Ihor Sofiichenko <asoluter@gmail.com>" \
      Name="telegram-cli" \
      Version=1.0

# see https://github.com/vysheng/tg/

RUN \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
    apt-get -y install apt-transport-https && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/*

RUN \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
    apt-get -y install \
      bash-completion \
      netcat-traditional \
      ssl-cert \
  && \
  cat /etc/bash_completion | tee /etc/bash.bashrc && \
  DEBIAN_FRONTEND=noninteractive \
    apt-get -y upgrade && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/* && \
  rm -v \
    /etc/ssl/certs/ssl-cert-snakeoil.pem \
    /etc/ssl/private/ssl-cert-snakeoil.key \
  && \
  mkdir -vp /var/log/symlinks/dev/ && \
  mkdir -vp /var/log/symlinks/proc/self/fd && \
  ln -s /dev/stdout /var/log/symlinks/dev/stdout && \
  ln -s /dev/stderr /var/log/symlinks/dev/stderr && \
  ln -s /proc/self/fd/1 /var/log/symlinks/proc/self/fd/1 && \
  ln -s /proc/self/fd/2 /var/log/symlinks/proc/self/fd/2

RUN \
  apt-get update && \
  DEBIAN_FRONTEND=noninteractive \
    apt-get install -y \
      build-essential \
      git \
      luajit \
      luarocks \
      libreadline-dev \
      libconfig-dev \
      libssl-dev \
      lua5.2 \
      liblua5.2-dev \
      libevent-dev \
      libjansson-dev \
      libpython-dev \
  && \
  git clone --recursive https://github.com/asoluter/tg.git /tg && \
  cd /tg && \
  ./configure && \
  cd /tg && \
  make && \
  mv -v /tg/bin/* /usr/bin/ && \
  mkdir -vp /etc/telegram-cli/ && \
  mv -v /tg/tg-server.pub /etc/telegram-cli/server.pub && \
  rm -rf /tg/ \
  && \
  DEBIAN_FRONTEND=noninteractive \
    apt-get purge -y --auto-remove \
      build-essential \
      git \
  && \
  apt-get clean && \
  rm -rf /var/lib/apt/lists/

ENTRYPOINT [ "/usr/bin/telegram-cli" ]