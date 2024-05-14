FROM ubuntu:22.04

SHELL ["/bin/bash", "-c"]

ENV RCUTILS_COLORIZED_OUTPUT=1
ENV CMAKE_BUILD_TYPE=RelWithDebInfo


# Install required packages
RUN --mount=type=cache,sharing=locked,target=/var/cache/apt \
    --mount=type=cache,sharing=locked,target=/var/lib/apt \
    --mount=type=cache,sharing=locked,target=/root/.cache \
    apt-get update \
    && apt-get upgrade -y \
    && apt-get install -qqy --no-install-recommends \
    # general dependencies
    bash-completion \
    build-essential \
    clang-format \
    cmake \
    gdb \
    git \
    htop \
    nano \
    iputils-ping \
    net-tools \
    sudo \
    wget \
    vim \
    # python dependencies
    python-is-python3 \
    python3-flake8 \
    python3-pip \
    python3-setuptools \
    && python -m pip install -U \
    argcomplete \
    pre-commit

RUN --mount=type=cache,sharing=locked,target=/var/cache/apt \
    --mount=type=cache,sharing=locked,target=/var/lib/apt \
    apt-get update -y && DEBIAN_FRONTEND=noninteractive apt-get install -qqy --no-install-recommends \
    build-essential \
    ca-certificates \
    cmake \
    coinor-libipopt-dev \
    git \
    libassimp-dev \
    libboost-all-dev \
    libeigen3-dev \
    liboctomap-dev \
    libqhull-dev \
    libtinyxml-dev \
    liburdfdom-dev \
    python-is-python3 \
    python3-dev \
    python3-numpy \
    python3-scipy

WORKDIR /src
ADD install_deps.sh .
RUN ./install_deps.sh /usr/local

# Create the user
RUN groupadd --gid 1000 arthur \
    && useradd --uid 1000 --gid 1000 -m arthur \
    && echo arthur ALL=\(root\) NOPASSWD:ALL > /etc/sudoers.d/arthur \
    && chmod 0440 /etc/sudoers.d/arthur

# Keep bash history when docker is rebuilt
RUN mkdir /commandhistory \
    && touch /commandhistory/.bash_history \
    && chown -R arthur /commandhistory \
    && echo "export PROMPT_COMMAND='history -a' && export HISTFILE=/commandhistory/.bash_history" >> "/home/arthur/.bashrc"

USER arthur
WORKDIR /home/arthur/workspace
