FROM elixir:1.4.2

## ensure locale is set during build
ENV LANG C.UTF-8
ENV TERM "xterm-256color"
ENV PATH $PATH:/root/.local/bin
CMD fish

RUN apt-get update 

# Core
RUN apt-get install -y --no-install-recommends \
    zlib1g-dev \
    libtinfo-dev \
    libsqlite3-0 \
    libsqlite3-dev \
    ca-certificates \
    g++ \
    git \ 
    vim \
    tmux \
    curl \
    wget \
    htop \
    glib-2.0 \
    silversearcher-ag

# Volume
RUN mkdir /data
VOLUME /data

# Dotfiles
WORKDIR /root
RUN git clone --recursive https://github.com/sato-s/vimrc.git
RUN pwd
RUN ls -la vimrc
RUN cp -rfp ~/vimrc/.vimrc ./
RUN cp -rfp ~/vimrc/.gitconfig ./
RUN cp -rfp ~/vimrc/.tmux.conf ./
RUN cp -rfp ~/vimrc/.vim ./

