FROM fedora:35

ARG IMAGE_USER=vscode
ARG IMAGE_RUST_VERSION=stable
ARG IMAGE_NODE_VERSION='16.15.0'

RUN dnf upgrade -y \
    && dnf install -y findutils nano git zsh curl make perl\
    && useradd -G wheel -s /usr/bin/zsh $IMAGE_USER \
    && sed -e 's/^%wheel/#%wheel/g' -e 's/^# %wheel/%wheel/g' -i /etc/sudoers

USER $IMAGE_USER

# Tools
RUN curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh | sh

# Node.js
RUN curl -fsSL https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.1/install.sh | sh \
    && . ~/.nvm/nvm.sh \
    && nvm install --no-progress $IMAGE_NODE_VERSION \
    && sed -e "s/^plugins=(/plugins=(npm /g" -i ~/.zshrc

ENV PATH="${PATH}:~/.nvm/versions/node/v${IMAGE_NODE_VERSION}/bin/"

# Rust
RUN curl -fsSL https://sh.rustup.rs | sh -s -- -y \
    && ~/.cargo/bin/rustup install $IMAGE_RUST_VERSION

ENV PATH="${PATH}:/home/$IMAGE_USER/.cargo/bin/"

# Setup wasm as target
RUN rustup target add wasm32-unknown-unknown

RUN rustup install nightly && rustup target add wasm32-unknown-unknown --toolchain nightly

# c++
RUN sudo dnf install gcc-c++ -y

RUN cargo install cargo-generate --features vendored-openssl

WORKDIR /home/${IMAGE_USER}