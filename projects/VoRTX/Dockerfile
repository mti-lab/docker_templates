FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu20.04

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

# Install Miniconda
RUN curl -so /miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && chmod +x /miniconda.sh \
 && /miniconda.sh -b -p /miniconda \
 && rm /miniconda.sh

ENV PATH=/miniconda/bin:$PATH

# Create Python3.9 enviroment
RUN conda install -y conda-build \
 && conda create -n vortx python=3.9 -y \
 && conda clean -ya

ENV CONDA_DEFAULT_ENV=vortx
ENV CONDA_PREFIX=/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Install dependencies
## pytorch
RUN conda install pytorch torchvision cudatoolkit=11.3 -c pytorch
## others
RUN pip install \
  pytorch-lightning==1.5 \
  scikit-image==0.18 \
  numba \
  pillow \
  wandb \
  tqdm \
  open3d \
  pyrender \
  ray \
  trimesh \
  pyyaml \
  matplotlib \
  black \
  pycuda \
  opencv-python \
  imageio

## torchsparse
RUN pip install git+https://github.com/mit-han-lab/torchsparse.git@v1.4.0 

## VoRTX
RUN git clone https://github.com/noahstier/vortx.git
RUN pip install -e vortx/.

# Others
RUN echo "export PATH=$CONDA_PREFIX/bin:$PATH" >> /etc/profile
RUN echo "export PYTHONPATH=/miniconda/envs/lib/python3.9/site-packages:$PYTHONPATH" >> /etc/profile
RUN echo "export NCCL_LL_THRESHOLD=0" >> /etc/profile
RUN echo "umask 002" >> /etc/profile

ENV NCCL_LL_THRESHOLD=0