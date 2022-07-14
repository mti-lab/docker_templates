FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu20.04

ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

RUN apt-get update && apt-get install -y \
    cmake \
    wget \
    curl \
    git \
    libopencv-dev \
    vim \
    unzip \
    && rm -rf /var/lib/apt/lists/*


# Install Miniconda
RUN curl -so /miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
 && chmod +x /miniconda.sh \
 && /miniconda.sh -b -p /miniconda \
 && rm /miniconda.sh

ENV PATH=/miniconda/bin:$PATH

# Create NeRF enviroment
RUN git clone https://github.com/sxyu/svox2.git
RUn conda env create -f svox2/environment.yml

# Set up conda default environment, etc.
ENV CONDA_DEFAULT_ENV=svox2
ENV CONDA_PREFIX=/miniconda/envs/$CONDA_DEFAULT_ENV
ENV PATH=$CONDA_PREFIX/bin:$PATH
ENV CONDA_AUTO_UPDATE_CONDA=false

# pip
RUN pip install \
  gdown

# Others
RUN echo "export PATH=$CONDA_PREFIX/bin:$PATH" >> /etc/profile
RUN echo "export PYTHONPATH=/miniconda/envs/lib/python3.7/site-packages:$PYTHONPATH" >> /etc/profile
RUN echo "export NCCL_LL_THRESHOLD=0" >> /etc/profile
RUN echo "umask 002" >> /etc/profile

ENV NCCL_LL_THRESHOLD=0
