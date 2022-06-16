# Get Template Docker Image 
## In this case, the following environments are preinstalled
### OS : Ubuntu 18.04
### CUDA : 11.3.0
### cuDNN : 8
FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu18.04

# Set TimeZone
ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

# Install packages
## Alphabetical order is better
## You should remove unnecessary packages
RUN apt-get update && apt-get install -y \
    curl \
    git \
    sudo \
    unzip \
    vim \
    wget \
    zip \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/*


# Install Miniconda
RUN curl -so /miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && chmod +x /miniconda.sh \
 && /miniconda.sh -b -p /miniconda \
 && rm /miniconda.sh

ENV PATH=/miniconda/bin:$PATH

# Create Python3.9 enviroment
## You can name the environment whatever you like,
## and you can use any Python version you like,
## BUT, you will need to rename the configuration such as CONDA_DEFAULT_ENV
RUN conda install -y conda-build \
 && conda create -n py39 python=3.9 -y \
 && conda clean -ya

# Set up conda default environment, etc.
ENV CONDA_DEFAULT_ENV=py39
ENV CONDA_PREFIX=/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# Install Python dependencies
## With Conda (Recommendation)
### Pytorch
### Need to install same version with CUDA
RUN conda install pytorch torchvision cudatoolkit=11.3 -c pytorch
## With Pip
RUN pip install \
  cupy-cuda113


# Others
## Possibly not necessary
RUN echo "export PATH=$CONDA_PREFIX/bin:$PATH" >> /etc/profile
RUN echo "export PYTHONPATH=/miniconda/envs/lib/python3.9/site-packages:$PYTHONPATH" >> /etc/profile
RUN echo "export NCCL_LL_THRESHOLD=0" >> /etc/profile
RUN echo "umask 002" >> /etc/profile

ENV NCCL_LL_THRESHOLD=0
