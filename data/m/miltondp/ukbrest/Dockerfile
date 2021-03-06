FROM continuumio/miniconda3  
MAINTAINER Milton Pividori <miltondp@gmail.com>  
  
# Setup conda environment  
COPY environment.yml /opt/  
RUN conda env update -n root -f /opt/environment.yml \  
&& conda clean --all  
  
# Docker repository for PostgreSQL, install client programs  
RUN DEBIAN_FRONTEND=noninteractive \  
apt-get update && apt-get install -y --no-install-recommends \  
postgresql-client-9.6 \  
&& apt-get clean \  
&& rm -rf /var/lib/apt/lists/*  
  
# Compile and install bgenix  
RUN export DEBIAN_FRONTEND=noninteractive \  
&& apt-get update && apt-get install -y --no-install-recommends \  
build-essential zlib1g-dev libbz2-dev mercurial \  
&& cd /tmp \  
&& hg clone https://gavinband@bitbucket.org/gavinband/bgen -u master \  
&& cd /tmp/bgen \  
&& export PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin"
\  
&& ./waf configure --prefix=/usr/local \  
&& ./waf \  
&& ./build/test/unit/test_bgen \  
&& ./waf install \  
&& apt-get remove -y build-essential zlib1g-dev libbz2-dev mercurial \  
&& apt-get autoremove -y \  
&& apt-get clean \  
&& rm -rf /var/lib/apt/lists/*  
  
# Copy ukbrest code  
ENV UKBREST_GENOTYPE_PATH="/var/lib/genotype"  
ENV UKBREST_PHENOTYPE_PATH="/var/lib/phenotype"  
COPY ukbrest /opt/ukbrest  
ENV PYTHONPATH="/opt"  
WORKDIR /opt  
  
COPY docker/start.py /opt/  
ENV WEB_CONCURRENCY=4  
ENV GUNICORN_CMD_ARGS="--log-file=- -k eventlet --timeout 10000 -b
0.0.0.0:5000"  
EXPOSE 5000  
ENTRYPOINT ["/opt/start.py"]  
CMD [""]  

