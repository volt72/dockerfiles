FROM ubuntu:xenial  
MAINTAINER ViViDboarder <vividboarder@gmail.com>  
  
RUN apt-get update \  
&& apt-get install -y --no-install-recommends \  
software-properties-common python-software-properties \  
&& add-apt-repository ppa:duplicity-team/ppa \  
&& apt-get update \  
&& apt-get install -y --no-install-recommends \  
cron \  
duplicity \  
lftp \  
ncftp \  
openssh-client \  
python-cloudfiles \  
python-gdata \  
python-oauthlib \  
python-paramiko \  
python-pexpect \  
python-pip \  
python-setuptools \  
python-swiftclient \  
python-urllib3 \  
rsync \  
tahoe-lafs \  
&& pip install -U boto b2 \  
&& apt-get autoremove -y python-pip \  
&& apt-get clean \  
&& rm -rf /var/apt/lists/*  
  
VOLUME /root/.cache/duplicity  
VOLUME /backups  
VOLUME /var/lock/duplicity  
  
ENV BACKUP_DEST="file:///backups"  
ENV BACKUP_NAME="backup"  
ENV PATH_TO_BACKUP="/data"  
ENV PASSPHRASE="Correct.Horse.Battery.Staple"  
ENV FLOCK_WAIT=60  
# Cron schedules  
ENV CRON_SCHEDULE=""  
ENV FULL_CRON_SCHEDULE=""  
ENV VERIFY_CRON_SCHEDULE=""  
ADD backup.sh /  
ADD restore.sh /  
ADD start.sh /  
ADD verify.sh /  
  
CMD [ "/start.sh" ]  

