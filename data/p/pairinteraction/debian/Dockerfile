FROM debian:stretch  
MAINTAINER Henri Menke <pairinteraction@henrimenke.de>  
ENV DEBIAN_FRONTEND noninteractive  
RUN apt-get update \  
&& apt-get install --no-install-recommends -y \  
cmake build-essential git \  
libeigen3-dev pyqt5-dev-tools \  
libboost-filesystem-dev libboost-program-options-dev \  
libboost-serialization-dev libboost-system-dev libboost-test-dev \  
libgsl-dev \  
libsqlite3-dev sqlite3 \  
libzmq3-dev \  
swig python3 python3-dev \  
python3-numpy python3-scipy python3-zmq \  
dpkg rpm \  
doxygen graphviz ca-certificates \  
python3-sphinx python3-numpydoc python3-sphinx-rtd-theme \  
python3-nbconvert pandoc ipython3 \  
&& apt-get clean \  
&& rm -rf /var/lib/apt/lists/*  

