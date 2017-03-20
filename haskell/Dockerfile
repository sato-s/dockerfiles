FROM ubuntu:16.10

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
    silversearcher-ag \
    fish

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

# Haskell
RUN apt-get install -y --no-install-recommends \
    pkg-config \
    ghc \
    libcairo2-dev \
    libpango1.0-dev \
    gtk+-3.0 \
    libwebkitgtk-3.0-dev
RUN curl -sSL https://get.haskellstack.org/ | sh
RUN stack setup

# Rbenv
RUN apt-get install -y --force-yes \
    build-essential \
    zlib1g-dev \
    libssl-dev \
    libreadline-dev \
    libyaml-dev \
    libxml2-dev \
    libxslt-dev

# Install rbenv and ruby-build
RUN git clone https://github.com/sstephenson/rbenv.git /root/.rbenv
RUN git clone https://github.com/sstephenson/ruby-build.git /root/.rbenv/plugins/ruby-build
RUN /root/.rbenv/plugins/ruby-build/install.sh
ENV PATH /root/.rbenv/bin:$PATH
RUN echo 'eval "$(rbenv init -)"' >> /etc/profile.d/rbenv.sh # or /etc/profile
RUN echo 'eval "$(rbenv init -)"' >> .bashrc


# Install multiple versions of ruby
ENV CONFIGURE_OPTS --disable-install-doc
ADD ./versions.txt /root/versions.txt
RUN xargs -L 1 rbenv install < /root/versions.txt

# Install Bundler for each version of ruby
RUN echo 'gem: --no-rdoc --no-ri' >> /.gemrc
RUN bash -l -c 'for v in $(cat /root/versions.txt); do rbenv global $v; gem install bundler; done'
