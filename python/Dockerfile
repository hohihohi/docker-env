# Reference: https://qiita.com/sugiyama404/items/3f28f06008f15d4f4ae7
# Common arguments for some stages
ARG PYTHON_VERSION=3.12 # python version
ARG PYTHON_BASE_IMAGE=python # python base image

# ---------- poetry container ----------
FROM ${PYTHON_BASE_IMAGE}:${PYTHON_VERSION} AS withpoetry

# Arguments
ARG POETRY_VERSION=1.8.3
ARG USER_UID=1000
ARG USER_GID=${USER_UID}

# Environment variables
ENV USERNAME=docker
ENV PYTHONUNBUFFERED=true
ENV HOME=/home/${USERNAME}
# The default binary location for pipx-installed apps is ~/.local/bin
ENV PIPX_BIN_DIR=${HOME}/.local/bin
ENV PATH=${PIPX_BIN_DIR}:${PATH}
ENV POETRY_VIRTUALENVS_IN_PROJECT=true

USER root:root

RUN set -eux \
    # Install apt packages
    && apt-get update \
    && apt-get install -y --no-install-recommends build-essential pipx sudo \
    && apt-get -y clean \
    && rm -rf /var/lib/apt/lists/* \
        # Create a non-root user
    ## https://code.visualstudio.com/remote/advancedcontainers/add-nonroot-user
    && groupadd --gid ${USER_GID} ${USERNAME} \
    && useradd --uid ${USER_UID} --gid ${USER_GID} -m ${USERNAME} \
    ## [Optional] Add sudo support. Omit if you don't need to install software after connecting.
    && echo ${USERNAME} ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

USER ${USERNAME}
WORKDIR ${HOME}
# Install pipx
## https://github.com/pypa/pipx?tab=readme-ov-file#on-linux
RUN pipx ensurepath \
    && pipx completions \
    # Install packages for development tools
    && pipx install poetry==${POETRY_VERSION}

# ---------- dev container ----------
FROM withpoetry AS dev
USER ${USERNAME}
WORKDIR ${HOME}
COPY  pyproject.toml ${HOME}/
COPY  poetry.lock ${HOME}/
RUN poetry install