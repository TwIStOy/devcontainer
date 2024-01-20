FROM --platform=linux/amd64 rust as rustbuilder

RUN cargo install ripgrep
RUN cargo install bat

FROM --platform=linux/amd64 ubuntu:24.04 as cmakebuilder

RUN apt update && apt install -y --no-install-recommends ca-certificates curl git g++ make \
    libjemalloc-dev libz-dev make pkg-config libtool libtool-bin m4 autoconf unzip libncursesw5-dev \
    libncurses-dev libssl-dev wget gettext python3-pip python3-venv cmake && \
    apt clean && rm -rf /var/lib/apt/lists/*

# install neovim
RUN cd /tmp && wget https://github.com/neovim/neovim/archive/refs/heads/master.zip && \
    unzip master.zip && cd neovim-master && \
    make CMAKE_BUILD_TYPE=Release && make install && rm -rf /tmp/neovim-master.zip && \
    rm -rf /tmp/neovim-master

FROM --platform=linux/amd64 ubuntu:24.04 as llvm-stage

RUN apt update && apt install -y --no-install-recommends xz-utils curl ca-certificates
RUN cd /tmp && curl -SL https://github.com/llvm/llvm-project/releases/download/llvmorg-17.0.6/clang+llvm-17.0.6-x86_64-linux-gnu-ubuntu-22.04.tar.xz -# -o llvm.tar.xz && \
    mkdir llvm-17/ && tar xf ./llvm.tar.xz -C llvm-17/ --strip-components=1

FROM --platform=linux/amd64 ubuntu:24.04 as node-stage

RUN apt update && apt install -y --no-install-recommends xz-utils curl ca-certificates
RUN cd /tmp && curl -SL https://nodejs.org/dist/v20.11.0/node-v20.11.0-linux-x64.tar.xz -# -o node.tar.xz && \
    mkdir node/ && tar xf ./node.tar.xz -C node/ --strip-components=1

FROM --platform=linux/amd64 ubuntu:24.04

RUN apt update && apt install -y --no-install-recommends ca-certificates curl git g++ make \
    libjemalloc-dev libz-dev make pkg-config libtool libtool-bin m4 autoconf unzip libncursesw5-dev \
    libncurses-dev libssl-dev wget gettext python3-pip python3-venv && \
    apt clean && rm -rf /var/lib/apt/lists/*

# install cmake
RUN cd /tmp && wget "https://github.com/Kitware/CMake/releases/download/v3.28.1/cmake-3.28.1.tar.gz" && \
    tar xf cmake-3.28.1.tar.gz && cd cmake-3.28.1 && \
    ./bootstrap && make -j8 && make install && rm -rf /tmp/cmake-3.28.1.tar.gz && \
    rm -rf /tmp/cmake-3.28.1

COPY --from=rustbuilder /usr/local/cargo/bin/rg /usr/local/bin/rg
COPY --from=rustbuilder /usr/local/cargo/bin/bat /usr/local/bin/bat
COPY --from=cmakebuilder /usr/local/bin/nvim /usr/local/bin/nvim
COPY --from=cmakebuilder /usr/local/share/nvim /usr/local/share/nvim
COPY --from=llvm-stage /tmp/llvm-17/bin/ /usr/bin/
COPY --from=llvm-stage /tmp/llvm-17/lib/ /usr/lib/
COPY --from=node-stage /tmp/node/bin/ /usr/bin/
COPY --from=node-stage /tmp/node/lib/ /usr/lib/
COPY --from=node-stage /tmp/node/include/ /usr/include/
COPY --from=node-stage /tmp/node/share/ /usr/share/

ENTRYPOINT ["/bin/bash"]

# RUN apt install -y fish
# 
# SHELL ["/bin/bash", "-c"]
# 
# # install fnm && nodejs
# RUN curl -fsSL https://fnm.vercel.app/install | bash -s -- --install-dir "/opt/fnm" --skip-shell && \
#     ln -s /opt/fnm/fnm /usr/bin/ && chmod +x /usr/bin/fnm && \
#     fnm -V
# 
# RUN /bin/bash -c "source /etc/bash.bashrc && fnm install --fnm-dir /opt/fnm 16.14"
# RUN /bin/bash -c "source /etc/bash.bashrc && fnm alias --fnm-dir /opt/fnm default 16.14" && \
#     /bin/bash -c 'source /etc/bash.bashrc && /bin/ln -s "/opt/fnm/aliases/default/bin/node" /usr/bin/node' && \
#     /bin/bash -c 'source /etc/bash.bashrc && /bin/ln -s "/opt/fnm/aliases/default/bin/npm" /usr/bin/npm' && \
#     node -v && \
#     npm -v && \
#     npm install -g neovim
# 
