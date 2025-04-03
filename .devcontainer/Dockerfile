## Changed some stuff.

FROM node:22-bookworm

# Set environment variables for CUDA/cuDNN
ENV CUDA_VERSION=11.2
ENV CUDNN_VERSION=8.1.0.77
ENV NVIDIA_VISIBLE_DEVICES=all
ENV NVIDIA_DRIVER_CAPABILITIES=compute,utility
ENV LD_LIBRARY_PATH="/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:/usr/lib/x86_64-linux-gnu:${LD_LIBRARY_PATH}"

ARG CUDNN_KEYRING_URL=https://developer.download.nvidia.com/compute/cuda/repos/debian10/x86_64/cuda-keyring_1.1-1_all.deb
ARG CUDNN_KEYRING_SHA256=78c8c918c53dd0e832e31f3917316a7381fb6e8c63a01b0fdc92a94a09a2d3f2

# Add NVIDIA package repositories
RUN curl -fsSL $CUDNN_KEYRING_URL -o cuda-keyring.deb && \
    echo "$CUDNN_KEYRING_SHA256  cuda-keyring.deb" | sha256sum -c - && \
    dpkg -i cuda-keyring.deb && \
    rm cuda-keyring.deb

# Install CUDA
RUN --mount=type=cache,target=/var/cache/apt \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    cuda-libraries-11-2 && \
    ldconfig

# Install cuDNN
RUN curl -O https://developer.download.nvidia.com/compute/redist/cudnn/v8.1.0/Ubuntu20_04-x64/libcudnn8_8.1.0.77-1+cuda11.2_amd64.deb && dpkg -i libcudnn8_8.1.0.77-1+cuda11.2_amd64.deb && rm libcudnn8_8.1.0.77-1+cuda11.2_amd64.deb && rm -rf /var/lib/apt/lists/*

# Install system dependencies and dev tools
RUN --mount=type=cache,target=/var/cache/apt \
    apt-get update && apt-get install -y \
    curl \
    wget \
    git \
    sudo \
    build-essential \
    htop \
    jq \
    nano \
    less \
    libglvnd-dev \
    pkg-config \
    zsh \
    postgresql-client

# Install Oh My Zsh for user 'coder'
RUN useradd -ms /bin/zsh coder && \
    echo "coder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \

# Install Prisma CLI
RUN npm install -g prisma

# VS Code Server setup placeholder (Coder will handle VS Code support)

# Set working directory
WORKDIR /workspace

# Switch to coder user
USER coder

# Expose required ports
EXPOSE 3000 9229

CMD ["zsh"]
