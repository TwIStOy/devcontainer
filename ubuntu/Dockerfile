FROM amd64/ubuntu:22.04

RUN apt update && apt install -y --no-install-recommends ca-certificates curl git g++ make \
    libjemalloc-dev libz-dev make pkg-config libtool libtool-bin m4 autoconf unzip libncursesw5-dev \
    libncurses-dev libssl-dev wget

# install rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# install tools
RUN /root/.cargo/bin/cargo install ripgrep
RUN /root/.cargo/bin/cargo install bat

# install cmake
RUN cd /tmp && wget "https://github.com/Kitware/CMake/releases/download/v3.20.1/cmake-3.20.1.tar.gz" && \
    tar xf cmake-3.20.1.tar.gz && cd cmake-3.20.1 && \
    ./bootstrap && make && make install && rm -rf /tmp/cmake-3.20.1.tar.gz && \
    rm -rf /tmp/cmake-3.20.1

