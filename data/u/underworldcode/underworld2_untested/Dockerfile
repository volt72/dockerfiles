FROM
underworldcode/base@sha256:59e242b34ea610784838232b93b81799828a3c59a25946e01129b71aae26ee3d  
MAINTAINER https://github.com/underworldcode/  
  
# set working directory to /opt, and install underworld files there.  
WORKDIR /opt  
ENV UW2_DIR /opt/underworld2  
RUN mkdir $UW2_DIR  
ENV PYTHONPATH $PYTHONPATH:$UW2_DIR  
ENV NB_WORK /workspace  
  
# add default user jovyan and change permissions on NB_WORK  
ENV NB_USER jovyan  
RUN useradd -m -s /bin/bash -N jovyan  
  
# copy this file over so that no password is required  
COPY docs/development/docker/underworld2_untested/jupyter_notebook_config.json
/home/$NB_USER/.jupyter/jupyter_notebook_config.json  
  
# install lavavu  
RUN pip install --no-cache-dir lavavu  
  
# COPY UW  
#COPY --chown=jovyan:users . $UW2_DIR/ # unfortunately, the version of docker
at docker cloud does not support chown yet.  
COPY . $UW2_DIR/  
  
# get underworld, compile, delete some unnecessary files, trust notebooks,
copy to workspace  
RUN cd underworld2/libUnderworld && \  
./configure.py --with-debugging=0 && \  
./compile.py && \  
rm -fr h5py_ext && \  
rm .sconsign.dblite && \  
rm -fr .sconf_temp && \  
cd build && \  
rm -fr libUnderworldPy && \  
rm -fr StGermain && \  
rm -fr gLucifer && \  
rm -fr Underworld && \  
rm -fr StgFEM && \  
rm -fr StgDomain && \  
rm -fr PICellerator && \  
rm -fr Solvers && \  
find $UW2_DIR/docs -name \\*.ipynb -print0 | xargs -0 jupyter trust && \  
cd ../../docs/development/api_doc_generator/ && \  
sphinx-build . ../../api_doc && \  
mkdir $NB_WORK && \  
rsync -av $UW2_DIR/docs/. $NB_WORK && \  
cd /opt/underworld2 && \  
find . -name \\*.os |xargs rm -f && \  
cat .git/refs/heads/* > build_commit.txt && \  
env > build_environment.txt && \  
rm -fr .git && \  
chown -R $NB_USER:users $NB_WORK $UW2_DIR /home/$NB_USER  
  
# kiwisolver hack, and pipe down numpy warnings  
RUN sed -i "1i import kiwisolver as _ks;import warnings as
_warnings;_warnings.filterwarnings(\"ignore\")"
/opt/underworld2/underworld/__init__.py  
  
# expose notebook port  
EXPOSE 8888  
# expose glucifer port  
EXPOSE 9999  
# CHANGE USER  
USER $NB_USER  
ENV PYTHONPATH $PYTHONPATH:$UW2_DIR  
  
# setup symlink for terminal convenience  
RUN ln -s $NB_WORK /home/$NB_USER/  
  
# create a volume  
VOLUME $NB_WORK/user_data  
WORKDIR $NB_WORK  
  
# launch notebook  
CMD ["jupyter", "notebook", "--ip='*'", "--no-browser"]  
  

