FROM ubuntu:16.04  
LABEL description = "Minimal image for the NBIS reproducible research course."  
MAINTAINER "Rasmus Agren" rasmus.agren@scilifelab.se  
  
# Install Miniconda3 prerequisites, English language pack and fonts. Also  
# include vim for convenience  
RUN apt-get update && \  
apt-get install -y --no-install-recommends bzip2 curl ca-certificates
language-pack-en fontconfig vim  
RUN curl https://repo.continuum.io/miniconda/Miniconda3-4.3.31-Linux-x86_64.sh
-O && \  
bash Miniconda3-4.3.31-Linux-x86_64.sh -bf -p /opt/miniconda3/ && \  
rm Miniconda3-4.3.31-Linux-x86_64.sh  
  
# Add conda to PATH and set locale  
ENV PATH="/opt/miniconda3/bin:${PATH}"  
ENV LC_ALL en_US.UTF-8  
ENV LC_LANG en_US.UTF-8  
# Install git and nano from conda-forge  
RUN conda install -c conda-forge git nano && \  
conda clean --all  
  
# Use bash as shell  
SHELL ["/bin/bash", "-c"]  
  
# Set workdir  
WORKDIR /course  
  
# Open port for running Jupyter Notebook  
EXPOSE 8888  
# Start Bash shell by default  
CMD /bin/bash  

