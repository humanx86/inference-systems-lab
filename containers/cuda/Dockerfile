FROM nvidia/cuda:13.3.1-devel-ubuntu24.04@sha256:4ff859525f99de5782aa73607ce24219b07dddd48d12b97c1c301d7e1cfb0a87

ARG DEBIAN_FRONTEND=noninteractive

ARG USERNAME=ubuntu

# dev utilities only
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
        ca-certificates \
        curl \
        git \
        openssh-client \
        sudo \
        python3.12 \
        python3.12-dev \
        python3.12-venv \
        cmake \
        ninja-build \
        pkg-config \
        gdb \
        jq \
        ripgrep \
        less \
    && rm -rf /var/lib/apt/lists/*

# passwordless sudo for development
RUN echo "${USERNAME} ALL=(root) NOPASSWD:ALL" \
        > /etc/sudoers.d/${USERNAME} \
    && chmod 0440 /etc/sudoers.d/${USERNAME}

# isolate python from system
ENV VIRTUAL_ENV=/home/${USERNAME}/.venv
ENV PATH="${VIRTUAL_ENV}/bin:${PATH}"

RUN python3.12 -m venv "${VIRTUAL_ENV}" \
    && mkdir -p \
        /home/${USERNAME}/.cache/pip \
        /home/${USERNAME}/.cache/triton \
        /home/${USERNAME}/.cache/torch_extensions \
    && chown -R ${USERNAME}:${USERNAME} \
        "${VIRTUAL_ENV}" \
        /home/${USERNAME}/.cache

ENV CUDA_HOME=/usr/local/cuda
ENV PYTHONUNBUFFERED=1
ENV PIP_DISABLE_PIP_VERSION_CHECK=1

WORKDIR /workspaces

USER ${USERNAME}