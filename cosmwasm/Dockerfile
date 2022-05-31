FROM fedora:35

ARG IMAGE_USER=vscode
ARG IMAGE_RUST_VERSION=stable

RUN dnf upgrade -y \
    && dnf install -y findutils nano git zsh curl make\
    && useradd -G wheel -s /usr/bin/zsh $IMAGE_USER \
    && sed -e 's/^%wheel/#%wheel/g' -e 's/^# %wheel/%wheel/g' -i /etc/sudoers

USER $IMAGE_USER

# Tools
RUN curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh | sh

# Rust
RUN curl -fsSL https://sh.rustup.rs | sh -s -- -y \
    && ~/.cargo/bin/rustup install $IMAGE_RUST_VERSION

ENV PATH="${PATH}:/home/$IMAGE_USER/.cargo/bin/"

# Setup wasm as target
RUN rustup target add wasm32-unknown-unknown

# c++
RUN sudo dnf install gcc-c++ -y

WORKDIR /home/${IMAGE_USER}

# Go
RUN curl -LO https://go.dev/dl/go1.18beta1.linux-amd64.tar.gz

RUN tar -C /home/$IMAGE_USER -xzf go1.18beta1.linux-amd64.tar.gz

ENV PATH=$PATH:/home/$IMAGE_USER/go/bin

# wasmd
RUN git clone https://github.com/CosmWasm/wasmd.git \
    && cd wasmd \
    && git checkout v0.23.0 \
    && make install

# verify the installation
RUN wasmd version

# setup Go cli
RUN source <(curl -sSL https://raw.githubusercontent.com/CosmWasm/testnets/master/cliffnet-1/defaults.env)

# add wallets for testing
RUN yes '123456789\n123456789' | wasmd keys add wallet
RUN yes '123456789\n123456789' | wasmd keys add wallet2


RUN x=$(echo '123456789\n123456789' | wasmd keys show -a wallet) && json='{"denom":"upebble","address":"'$x'"}' && curl -X POST --header "Content-Type: application/json" --data "$json" https://faucet.cliffnet.cosmwasm.com/credit
RUN x=$(echo '123456789\n123456789' | wasmd keys show -a wallet) && json='{"denom":"upebble","address":"'$x'"}' && curl -X POST --header "Content-Type: application/json" --data "$json" https://faucet.cliffnet.cosmwasm.com/credit

# ENV NODE="--node $RPC"
# ENV TXFLAG="${NODE} --chain-id ${CHAIN_ID} --gas-prices 0.025upebble --gas auto --gas-adjustment 1.3"


