## Changed some stuff.

FROM node:22-bookworm

# Set environment variables for CUDA/cuDNN
ENV CUDA_VERSION=11.2 \
    CUDNN_VERSION=8.1.0.77 \
    NVIDIA_VISIBLE_DEVICES=all \
    NVIDIA_DRIVER_CAPABILITIES=compute,utility \
    LD_LIBRARY_PATH="/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:/usr/lib/x86_64-linux-gnu:${LD_LIBRARY_PATH}"

# Add NVIDIA package repositories
RUN curl -O https://developer.download.nvidia.com/compute/cuda/repos/debian10/x86_64/cuda-keyring_1.1-1_all.deb && \
    dpkg -i cuda-keyring_1.1-1_all.deb && \
    rm cuda-keyring_1.1-1_all.deb && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    cuda-libraries-11-2 && \
    curl -O https://developer.download.nvidia.com/compute/redist/cudnn/v8.1.0/Ubuntu20_04-x64/libcudnn8_8.1.0.77-1+cuda11.2_amd64.deb && \
    dpkg -i libcudnn8_8.1.0.77-1+cuda11.2_amd64.deb && \
    rm libcudnn8_8.1.0.77-1+cuda11.2_amd64.deb && \
    rm -rf /var/lib/apt/lists/* && \
    ldconfig && \

# Install system dependencies and dev tools
RUN apt-get update && apt-get install -y \
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
    postgresql-client \
    && rm -rf /var/lib/apt/lists/*

# Install Oh My Zsh for user 'coder'
RUN useradd -ms /bin/zsh coder && \
    echo "coder ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers && \
    su - coder -c "sh -c \"$(curl -fsSL https://raw.githubusercontent.com/ohmyzsh/ohmyzsh/master/tools/install.sh)\" --unattended"

# Set up kubectl, helm, minikube (latest versions)
RUN curl -LO https://storage.googleapis.com/kubernetes-release/release/$(curl -s https://storage.googleapis.com/kubernetes-release/release/stable.txt)/bin/linux/amd64/kubectl \
    && chmod +x kubectl && mv kubectl /usr/local/bin/ \
    && curl https://raw.githubusercontent.com/helm/helm/main/scripts/get-helm-3 | bash \
    && curl -Lo minikube https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64 \
    && chmod +x minikube && mv minikube /usr/local/bin/

# Install Prisma CLI
RUN npm install -g prisma

# VS Code Server setup placeholder (Coder will handle VS Code support)

# Set working directory
WORKDIR /workspace/ourmoney

# Switch to coder user
USER coder

# Expose required ports
EXPOSE 3000 9229

CMD ["zsh"]
