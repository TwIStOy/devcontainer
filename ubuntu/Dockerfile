FROM amd64/ubuntu:22.04

# replace mirrors
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list \
  && apt update

RUN apt install -y --no-install-recommends ca-certificates curl git g++

# install rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

RUN apt install -y --no-install-recommends libjemalloc-dev libz-dev make \
    pkg-config libtool libtool-bin m4 autoconf unzip libncursesw5-dev \
    libncurses-dev libssl-dev

# install tools
RUN /root/.cargo/bin/cargo install ripgrep fd-find bat exa stylua git-delta

# install cmake
RUN apt install -y --no-install-recommends wget
RUN cd /tmp && wget "https://github.com/Kitware/CMake/releases/download/v3.20.1/cmake-3.20.1.tar.gz" && \
    tar xf cmake-3.20.1.tar.gz && cd cmake-3.20.1 && \
    ./bootstrap && make && make install

