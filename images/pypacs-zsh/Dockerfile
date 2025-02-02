# Copyright (c) octanevz.
# Distributed under the terms of the Modified BSD License.
ARG REGISTRY=docker.io
ARG OWNER=octanevz
ARG TAG=v250202
ARG BASE_CONTAINER=octanevz/pypacs:${TAG}
FROM $BASE_CONTAINER

LABEL maintainer="octanevz <octanevz@gmx.net>"

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Install zsh
RUN apt-get update --yes && \
    apt-get upgrade -y && \
    apt-get install --yes --no-install-recommends \
    zsh && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

COPY ./files/zsh/.zshrc /etc/skel/.zshrc
COPY ./files/zsh/.p10k.zsh /etc/skel/.p10k.zsh

RUN chmod 644 /etc/skel/.zshrc && \
    chmod 644 /etc/skel/.p10k.zsh && \
    sed -i 's/\r$//' /etc/skel/.zshrc && \
    sed -i 's/\r$//' /etc/skel/.p10k.zsh

USER ${NB_UID}

# Install oh-my-zsh
RUN sh -c "$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)" "" --unattended && \
    git clone https://github.com/zsh-users/zsh-autosuggestions ${ZSH_CUSTOM:-~/.oh-my-zsh/custom}/plugins/zsh-autosuggestions && \
    git clone --depth=1 https://github.com/romkatv/powerlevel10k.git ${ZSH_CUSTOM:-$HOME/.oh-my-zsh/custom}/themes/powerlevel10k

COPY --chown=${NB_UID}:${NB_GID} ./files/zsh/.zshrc /home/${NB_USER}/.zshrc
COPY --chown=${NB_UID}:${NB_GID} ./files/zsh/.p10k.zsh /home/${NB_USER}/.p10k.zsh

USER root
RUN chmod 664 /home/${NB_USER}/.zshrc && \
    chmod 664 /home/${NB_USER}/.p10k.zsh && \
    sed -i 's/\r$//' /home/${NB_USER}/.zshrc && \
    sed -i 's/\r$//' /home/${NB_USER}/.p10k.zsh

ENV SHELL /usr/bin/zsh

USER ${NB_UID}
WORKDIR "${HOME}"
