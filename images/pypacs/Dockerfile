# Copyright (c) octanevz.
# Distributed under the terms of the Modified BSD License.
ARG REGISTRY=docker.io
ARG OWNER=octanevz
ARG BASE_CONTAINER=quay.io/jupyter/scipy-notebook:python-3.11
FROM $BASE_CONTAINER

LABEL maintainer="octanevz <octanevz@gmx.net>"

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Install zsh
RUN apt-get update --yes && \
    apt-get upgrade -y && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

# Download and install TA-Lib Core C Library
RUN curl -L -o ta-lib_0.6.4_amd64.deb https://github.com/ta-lib/ta-lib/releases/download/v0.6.4/ta-lib_0.6.4_amd64.deb \
    && dpkg -i ta-lib_0.6.4_amd64.deb \
    && rm ta-lib_0.6.4_amd64.deb

USER ${NB_UID}

# Install additional packages
RUN mamba install --yes \
    'plotly' \
    'pyscaffold' \
    'pyts' \
    'sktime-all-extras' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Install additional PIP packages
RUN pip install --no-cache-dir \
    'ccxt' \
    'mplfinance' \
    'TA-Lib'

# Install additional Python packages from GitHub
# RUN pip install --no-cache-dir \
#     'git+https://github.com/twopirllc/pandas-ta.git@development'
