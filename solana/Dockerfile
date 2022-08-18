
FROM ubuntu:20.04

ARG DEBIAN_FRONTEND=noninteractive
RUN apt-get update && apt-get install git curl zsh -y

RUN chsh -s $(which zsh)

RUN sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

RUN curl -fsSL https://sh.rustup.rs | sh -s -- -y

RUN curl https://raw.githubusercontent.com/creationix/nvm/master/install.sh | bash

RUN DEBIAN_FRONTEND=noninteractive apt install -y build-essential pkg-config libudev-dev libclang-dev libssl-dev cmake

