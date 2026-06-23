FROM debian:sid

ENV LANG=C.UTF-8 LC_ALL=C.UTF-8

RUN apt-get update && \
    apt-get -y dist-upgrade && \
    apt-get -y autopurge

RUN apt-get -y install curl git gnupg jq less man parallel patch ssh vim wget xz-utils

RUN apt-get -y install cmake ninja-build python3

ARG USERNAME
RUN useradd "${USERNAME}" -ms '/bin/bash'

USER "${USERNAME}"

RUN git clone 'https://github.com/emscripten-core/emsdk' "${HOME}/emsdk" && \
    cd "${HOME}/emsdk" && \
    ./emsdk install latest && \
    ./emsdk activate latest && \
    printf '\nEMSDK_QUIET=1 . ${HOME}/emsdk/emsdk_env.sh\n' >> "${HOME}/.bashrc"

USER root

ARG COMPOSE_PROJECT_NAME
COPY --chmod=644 --chown="${USERNAME}:${USERNAME}" '.vimrc' "/home/${USERNAME}"
COPY --chmod=755 'dotfiles_hook' '/usr/local/bin'
RUN install -d -o "${USERNAME}" -g "${USERNAME}" "/home/${USERNAME}/dotfiles" && \
    install -d -o "${USERNAME}" -g "${USERNAME}" "/workspaces/${COMPOSE_PROJECT_NAME}" && \
    install -d -o "${USERNAME}" -g "${USERNAME}" "/workspaces/alternative/${COMPOSE_PROJECT_NAME}" && \
    sed -i '/PS1.*\\\$/s/\\\$/\\n\0/' "/home/${USERNAME}/.bashrc"
