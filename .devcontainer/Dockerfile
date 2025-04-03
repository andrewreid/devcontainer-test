## Changed some stuff.

FROM node:22-bookworm

# Set environment variables for CUDA/cuDNN
ENV CUDA_VERSION=11.2 \
    CUDNN_VERSION=8.1.0.77 \
    NVIDIA_VISIBLE_DEVICES=all \
    NVIDIA_DRIVER_CAPABILITIES=compute,utility \
    LD_LIBRARY_PATH="/usr/local/cuda/lib64:/usr/local/cuda/extras/CUPTI/lib64:/usr/lib/x86_64-linux-gnu:${LD_LIBRARY_PATH}"

# Add NVIDIA package repositories
RUN apt-get update && apt-get install -y --no-install-recommends \
    gnupg \
    ca-certificates \
    curl && \
    curl -fsSL https://developer.download.nvidia.com/compute/cuda/repos/debian11/x86_64/3bf863cc.pub | gpg --dearmor -o /usr/share/keyrings/nvidia-archive-keyring.gpg && \
    echo "deb [signed-by=/usr/share/keyrings/nvidia-archive-keyring.gpg] https://developer.download.nvidia.com/compute/cuda/repos/debian11/x86_64 /" > /etc/apt/sources.list.d/cuda.list && \
    apt-get update && \
    apt-get install -y --no-install-recommends \
    cuda-toolkit-11-2 \
    libcudnn8=8.1.0.*-1+cuda11.2 \
    libcudnn8-dev=8.1.0.*-1+cuda11.2 && \
    ldconfig && \
    rm -rf /var/lib/apt/lists/*

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
