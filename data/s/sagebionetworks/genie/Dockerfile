FROM ubuntu:16.04  
RUN echo "deb http://cran.rstudio.com/bin/linux/ubuntu xenial/" | tee -a
/etc/apt/sources.list  
RUN gpg --keyserver keyserver.ubuntu.com --recv-key E084DAB9  
RUN gpg -a --export E084DAB9 | apt-key add -  
  
RUN apt-get update && apt-get install -y \  
bedtools \  
dos2unix \  
wget \  
python \  
python-pip \  
python-pandas \  
git \  
r-base \  
r-base-dev  
  
RUN pip install --upgrade pip  
RUN pip install synapseclient httplib2 pycrypto  
RUN pip install pandas numexpr --upgrade  
  
COPY installPackages.R /installPackages.R  
RUN Rscript /installPackages.R  
  
#install pandoc 1.19.2.1  
RUN wget
https://github.com/jgm/pandoc/releases/download/1.19.2.1/pandoc-1.19.2.1-1-amd64.deb  
RUN dpkg -i pandoc-1.19.2.1-1-amd64.deb  
  
WORKDIR /root/  
RUN git clone https://github.com/Sage-Bionetworks/Genie.git -b tempvalid  
RUN git clone https://github.com/cBioPortal/cbioportal.git  
  
WORKDIR /root/Genie  
RUN mkdir -p MDA/input MDA/staging MSK/input MSK/staging GRCC/input
GRCC/staging DFCI/input DFCI/staging JHU/input JHU/staging VICC/input
VICC/staging UHN/input UHN/staging NKI/input NKI/staging  
  
WORKDIR /root/Genie/processing_sage  
RUN wget
ftp://ftp.ensembl.org/pub/release-75/gtf/homo_sapiens/Homo_sapiens.GRCh37.75.gtf.gz  
RUN gunzip Homo_sapiens.GRCh37.75.gtf.gz  
RUN awk '$3 == "exon" {print}' Homo_sapiens.GRCh37.75.gtf > exon.gtf  
RUN awk '$3 == "gene" {print}' Homo_sapiens.GRCh37.75.gtf > gene.gtf  

