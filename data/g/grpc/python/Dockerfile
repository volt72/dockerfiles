FROM grpc/python:0.11  
RUN mkdir -p /usr/src/app  
WORKDIR /usr/src/app  
  
ONBUILD COPY requirements.txt /usr/src/app/  
ONBUILD RUN pip install --no-cache-dir -r requirements.txt  
  
ONBUILD COPY . /usr/src/app  

