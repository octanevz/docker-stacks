# Copyright (c) octanevz.
# Distributed under the terms of the Modified BSD License.
ARG REGISTRY=docker.io
ARG OWNER=octanevz
ARG BASE_CONTAINER=quay.io/jupyter/scipy-notebook
FROM $BASE_CONTAINER

LABEL maintainer="octanevz <octanevz@gmx.net>"

# Fix: https://github.com/hadolint/hadolint/wiki/DL4006
# Fix: https://github.com/koalaman/shellcheck/wiki/SC3014
SHELL ["/bin/bash", "-o", "pipefail", "-c"]

USER root

# Install zsh
RUN apt-get update --yes && \
    apt-get install --yes --no-install-recommends \
    zsh && \
    apt-get clean && rm -rf /var/lib/apt/lists/*

USER ${NB_UID}

# Install additional packages
RUN mamba install --yes \
    'libta-lib' \
    'plotly' \
    'pyts' \
    'sktime-all-extras' && \
    mamba clean --all -f -y && \
    fix-permissions "${CONDA_DIR}" && \
    fix-permissions "/home/${NB_USER}"

# Install additional PIP packages
RUN pip install --no-cache-dir \
    'ccxt' \
    'mplfinance' \
    'TA-Lib' \
    'tensorflow'

# Install additional Python packages from GitHub
RUN pip install --no-cache-dir \
    'git+https://github.com/twopirllc/pandas-ta.git@development'
