FROM nvidia/cuda:12.8.0-devel-ubuntu22.04

# Set environment variables
ENV DEBIAN_FRONTEND=noninteractive
ENV USER=user
ENV HOME=/home/$USER

# Install additional packages
RUN apt-get update && apt-get install -y \
    sudo curl wget git nano \
    build-essential ca-certificates && \
    rm -rf /var/lib/apt/lists/*

# Use Python 3.10 as default
RUN apt-get update && apt-get install -y \
    python3.10 python3-pip && \
    rm -rf /var/lib/apt/lists/*

# Install PyTorch 2.8.0 (CUDA 12.8)
RUN pip install torch==2.8.0 torchvision==0.23.0 torchaudio==2.8.0 \
    --index-url https://download.pytorch.org/whl/cu128

# Install FAISS (CPU version)
RUN pip install faiss-cpu

# Install PyTorch3D dependencies
RUN apt-get update && apt-get install -y \
    libjpeg-dev libpng-dev cmake && \
    rm -rf /var/lib/apt/lists/*

# Install PyTorch3D
RUN git clone https://github.com/facebookresearch/pytorch3d.git /tmp/pytorch3d && \
    cd /tmp/pytorch3d && \
    pip install . && \
    rm -rf /tmp/pytorch3d

# Install other Python packages
RUN pip install \
    tyro open_clip_torch wandb h5py openai hydra-core distinctipy ultralytics \
    dill open3d imageio natsort kornia rerun-sdk pyliblzfse pypng supervision \
    git+https://github.com/openai/CLIP.git

# Bug fixes in docker container (opencv and rerun-sdk)
RUN apt-get update && apt-get install -y \
    libgl1-mesa-glx libglib2.0-0 \
    libgtk-3-dev libxkbcommon-x11-0 vulkan-tools && \
    rm -rf /var/lib/apt/lists/*

# Create user and home directory
RUN useradd -m -d $HOME -s /bin/bash $USER && \
    echo "$USER:$USER" | chpasswd && \
    usermod -aG sudo $USER && \
    echo "$USER ALL=(ALL) NOPASSWD:ALL" >> /etc/sudoers

RUN mkdir -p $HOME/workspace

# Install Concept-Graphs
RUN git clone https://github.com/JorgeDFR/concept-graphs.git $HOME/workspace/concept-graphs && \
    cd $HOME/workspace/concept-graphs && \
    pip install -e .

COPY ./run_concept-graphs.sh $HOME/workspace/run_concept-graphs.sh
RUN chmod +x $HOME/workspace/run_concept-graphs.sh
RUN chown -R $USER:$USER $HOME

# Set working directory to home and default command
USER $USER
WORKDIR $HOME/workspace
CMD ["/bin/bash"]