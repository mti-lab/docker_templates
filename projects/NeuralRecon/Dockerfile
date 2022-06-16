FROM nvidia/cuda:10.2-cudnn7-devel-ubuntu18.04

ARG DEBIAN_FRONTEND=noninteractive

ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
    cmake \
    wget \
    curl \
    git \
    rsync \
    sudo \
    zip \ 
    vim \
    ssh \
    unzip \
    screen \
    openssh-server \
    pkg-config \
    build-essential \
    libboost-all-dev \
    libsuitesparse-dev \
    libfreeimage-dev \
    libgoogle-glog-dev \
    libgflags-dev \
    libglew-dev \
    freeglut3-dev \
    qt5-default \
    libxmu-dev \
    libxi-dev \
    libatlas-base-dev \
    libsuitesparse-dev \
    libcgal-dev \ 
    libeigen3-dev \
    libsparsehash-dev \
  && rm -rf /var/lib/apt/lists/*

# CUDA環境設定
RUN nvidia-smi
ENV FORCE_CUDA="1"

# Install Miniconda
RUN curl -so /miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && chmod +x /miniconda.sh \
 && /miniconda.sh -b -p /miniconda \
 && rm /miniconda.sh

ENV PATH=/miniconda/bin:$PATH

# Install dependencies
## NeuralRecon
RUN git clone https://github.com/zju3dv/NeuralRecon.git
RUN cd NeuralRecon \
 && conda install -y conda-build \
 && conda create -n neucon python=3.7 -y \
 && conda clean -ya

ENV CONDA_DEFAULT_ENV=neucon
ENV CONDA_PREFIX=/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# dependencies
RUN conda install pytorch==1.6.0 torchvision==0.7.0 cudatoolkit=10.2 -c pytorch

RUN pip install \
 ipython \
 ray \
 transforms3d \
 tqdm \
 opencv-python \
 numba \
 pycuda \
 tensorboardX \
 scikit-image \
 trimesh~=3.9.18 \
 yacs \
 h5py \
 loguru \
 gdown \ 
 pyrender~=0.1.45 \
 pyglet \
 open3d 

RUN pip install git+https://github.com/mit-han-lab/torchsparse.git@v1.4.0

# Others
RUN echo "export PATH=$CONDA_PREFIX/bin:$PATH" >> /etc/profile
RUN echo "export NCCL_LL_THRESHOLD=0" >> /etc/profile
RUN echo "umask 002" >> /etc/profile

ENV NCCL_LL_THRESHOLD=0