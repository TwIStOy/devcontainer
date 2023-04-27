FROM amd64/ubuntu:22.04

# replace mirrors
RUN sed -i 's@//.*archive.ubuntu.com@//mirrors.ustc.edu.cn@g' /etc/apt/sources.list \
  && apt update

RUN apt install -y --no-install-recommends ca-certificates curl git g++

# install rust
RUN curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh -s -- -y

# install tools
RUN /root/.cargo/bin/cargo install ripgrep fd-find bat exa delta stylua

