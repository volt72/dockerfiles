FROM tensorflow/tensorflow:1.0.0.2-gpu-py3  
  
RUN apt-get update && \  
apt-get install git -y  
  
#PROCESSING  
RUN pip install scoop  
  
#GRAPHING  
RUN pip install plotly && \  
# pip install python-igraph && \  
pip install seaborn && \  
pip install altair && \  
pip install git+https://github.com/jakevdp/JSAnimation.git  
  
#GEO  
RUN pip install Geohash && \  
pip install mplleaflet && \  
apt-get install libgeos-dev -y  
  
#TEXT PROCESSING  
RUN pip install textblob && \  
pip install git+git://github.com/amueller/word_cloud.git && \  
pip install toolz cytoolz  
  
#DATA  
RUN pip install h5py && \  
pip install pyexcel-ods && \  
pip install pandas-profiling && \  
pip install sklearn-pandas  
  
#IMAGE  
RUN pip install pydicom && \  
pip install \--trusted-host itk.org -f
https://itk.org/SimpleITKDoxygen/html/PyDownloadPage.html SimpleITK && \  
pip install scikit-image && \  
pip install opencv-python && \  
pip install ImageHash && \  
apt-get install libav-tools -y && \  
apt-get install imagemagick -y && \  
pip install git+https://github.com/danoneata/selectivesearch.git  
  
#LEARNING  
RUN apt-get install pandoc -y && pip install pypandoc && pip install deap && \  
pip install git+https://github.com/tflearn/tflearn.git && \  
pip install scipy && \  
pip install scikit-learn && \  
pip install tpot && \  
pip install heamy  
  
RUN cd /usr/local/src && mkdir keras && cd keras && \  
#keras  
git clone \--depth 1 https://github.com/fchollet/keras.git && \  
cd keras && python setup.py install && \  
#keras-rl  
cd /usr/local/src && mkdir keras-rl && cd keras-rl && \  
git clone \--depth 1 https://github.com/matthiasplappert/keras-rl.git && \  
cd keras-rl && python setup.py install && \  
# Keras likes to add a config file in a custom directory when it's first
imported. This doesn't work with our read-only filesystem, so we have it done
now  
python -c "from keras.models import Sequential" && \  
# Switch to TF backend  
sed -i 's/theano/tensorflow/' /root/.keras/keras.json && \  
# Re-run it to flush any more disk writes  
python -c "from keras.models import Sequential; from keras import backend;
print(backend._BACKEND)" && \  
# Keras reverts to /tmp from ~ when it detects a read-only file system  
mkdir -p /tmp/.keras && cp /root/.keras/keras.json /tmp/.keras  
  
#MISC  
RUN pip install wavio && \  
pip install trueskill  
  
#CLEANUP  
RUN rm -rf /root/.cache/pip/* && \  
apt-get autoremove -y && \  
apt-get clean && \  
rm -rf /usr/local/src/*  
  
RUN apt-get install supervisor -y  
  
COPY supervisord.conf /etc/supervisor/conf.d/supervisord.conf  
  
CMD ["/usr/bin/supervisord"]  

