FROM nvidia/cuda:11.3.0-cudnn8-devel-ubuntu18.04

ARG DEBIAN_FRONTEND=noninteractive

ENV TZ=Asia/Tokyo
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone

ENV PYTHON_VERSION=3.7.0
ENV OPENCV_VERSION=4.5.5.62

RUN apt-get update && apt-get install -y \
	curl \
	git \
	wget \
	ffmpeg \
	tk-dev \
	libxi-dev \
	libc6-dev \
	libbz2-dev \
	libffi-dev \
	libomp-dev \
	libssl-dev \
	zlib1g-dev \
	libcgal-dev \
	libgdbm-dev \
	libglew-dev \
	python3-dev \
	python3-pip \
	qtbase5-dev \
	checkinstall \
	libglfw3-dev \
	libeigen3-dev \
	libgflags-dev \
	libxrandr-dev \
	libopenexr-dev \
	libsqlite3-dev \
	libxcursor-dev \
	build-essential \
	libcgal-qt5-dev \
	libxinerama-dev \
	libboost-all-dev \
	libfreeimage-dev \
	libncursesw5-dev \
	libatlas-base-dev \
	libqt5opengl5-dev \
	libgoogle-glog-dev \
	libsuitesparse-dev \
	python3-setuptools \
	libreadline-gplv2-dev \
    libmetis-dev \
    libboost-program-options-dev \
    libboost-filesystem-dev  \
    libboost-graph-dev \
    libboost-regex-dev \
    libboost-system-dev \
    libboost-test-dev \
	&& apt autoremove -y \
	&& apt clean -y \
  	&& rm -rf /var/lib/apt/lists/*

# Install New Cmake
RUN mkdir -p /opt/cmake \
	&& cd /opt/cmake \
	&& wget https://cmake.org/files/v3.23/cmake-3.23.2.tar.gz \
	&& tar xvf cmake-3.23.2.tar.gz \
	&& cd cmake-3.23.2\
	&& ./bootstrap && make  && make install

# Colmap
## Ceres Solver
RUN cd /opt \
	&& wget http://ceres-solver.org/ceres-solver-2.1.0.tar.gz \
	&& tar -xvzf ceres-solver-2.1.0.tar.gz \
	&& mkdir ceres-bin \
	&& cd ceres-bin \
	&& cmake ../ceres-solver-2.1.0 \
	&& make -j3 \
	&& make install
## Colmap
RUN cd /opt \
	&& git clone https://github.com/colmap/colmap \
	&& cd colmap \
	&& git checkout 3.6 \
	&& mkdir -p build && cd build \
	&& cmake .. \
	&& make -j3 \
	&& make install \
	&& colmap -h


# Install Miniconda
#RUN cd /opt \
#	&& curl -so /miniconda.sh https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh \
#	&& chmod +x /miniconda.sh \
#	&& /miniconda.sh -b -p /miniconda \
#	&& rm /miniconda.sh
#ENV PATH=/miniconda/bin:$PATH

# Install dependencies
#RUN conda install -y conda-build \
#	&& conda create -n instant-ngp python=3.7 -y \
#	&& conda clean -ya

#ENV CONDA_DEFAULT_ENV=instant-ngp
#ENV CONDA_PREFIX=/miniconda/envs/$CONDA_DEFAULT_ENV
#ENV PATH=$CONDA_PREFIX/bin:$PATH
#ENV CONDA_AUTO_UPDATE_CONDA=false

# install python
RUN cd /opt \
	&& wget https://www.python.org/ftp/python/${PYTHON_VERSION}/Python-${PYTHON_VERSION}.tgz \
	&& tar xzf Python-${PYTHON_VERSION}.tgz \
	&& cd ./Python-${PYTHON_VERSION} \
	&& ./configure --enable-optimizations \
	&& make \
	&& checkinstall


# pip dependencies
RUN python3 -m pip install -U pip \
	&& pip3 --no-cache-dir install opencv-python==${OPENCV_VERSION} \ 
	&& pip3 --no-cache-dir install imageio-ffmpeg imageio

# CUDA
ENV PATH /usr/local/cuda-11.3/bin:$PATH
ENV LD_LIBRARY_PATH /usr/local/cuda-11.3/lib64:$LD_LIBRARY_PATH

# Setting will be changed by GPU
ENV TCNN_CUDA_ARCHITECTURES 80

RUN cd /opt \
	&& git clone --recursive https://github.com/NVlabs/instant-ngp.git \
	&& cd ./instant-ngp \
	&& pip3 --no-cache-dir install -r ./requirements.txt \
	&& mkdir build \
	&& cmake .  -B build \
	&& cmake --build build --config RelWithDebInfo -j3 --target all 

# Path to pyngp library
ENV PYTHONPATH /opt/instant-ngp/build/
# add
## TODO : GLウィンドウ開けるようにしたい
## LIBGL
ENV LIBGL_ALWAYS_INDIRECT 1