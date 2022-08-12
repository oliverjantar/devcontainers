FROM fedora:35

ARG IMAGE_USER=vscode
ARG IMAGE_NODE_VERSION='16.15.0'

RUN dnf upgrade -y \
    && dnf install -y findutils nano bzip2 git zsh curl gcc openssl-devel libudev-devel \
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

# Install c++
RUN sudo dnf install make automake gcc gcc-c++ kernel-devel -y

WORKDIR /home/${IMAGE_USER}

# Install go
RUN curl -LO https://golang.org/dl/go1.18.2.linux-amd64.tar.gz

RUN tar -C /home/$IMAGE_USER -xzf go1.18.2.linux-amd64.tar.gz

ENV GOROOT=/home/$IMAGE_USER/go
ENV GOPATH=/home/$IMAGE_USER/go/bin
ENV GO111MODULE=on

ENV PATH=$PATH:/home/$IMAGE_USER/go/bin

ENV PATH=$PATH:/home/$IMAGE_USER/.nvm/versions/node/v$IMAGE_NODE_VERSION/bin

# Install junod
RUN git clone https://github.com/CosmosContracts/juno \
    && cd juno \
    && git fetch \
    && git checkout v6.0.0 \
    && make install