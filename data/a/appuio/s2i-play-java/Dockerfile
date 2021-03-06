# extend the base image provided by OpenShift  
FROM openshift/base-centos7  
  
# set labels used in OpenShift to describe the builder image  
LABEL \  
io.k8s.description="Platform for building Java Play! applications" \  
io.k8s.display-name="java-play" \  
io.openshift.expose-services="9000:http" \  
io.openshift.tags="builder,java,play" \  
# location of the STI scripts inside the image.  
io.openshift.s2i.scripts-url=image://$STI_SCRIPTS_PATH  
  
# specify wanted versions of Java and SBT  
ENV JAVA_VERSION=1.8.0 \  
SBT_VERSION=0.13.16 \  
HOME=/opt/app-root/src \  
PATH=/opt/app-root/bin:$PATH  
  
# expose the default Play! port  
EXPOSE 9000  
# install Java and SBT  
RUN curl https://bintray.com/sbt/rpm/rpm > /etc/yum.repos.d/bintray-sbt-
rpm.repo && \  
yum install -y \  
java-${JAVA_VERSION}-openjdk \  
java-${JAVA_VERSION}-openjdk-devel \  
sbt-${SBT_VERSION} && \  
yum clean all -y  
  
# initialize SBT  
RUN sbt -ivy ${HOME}/.ivy2 -debug about  
  
# copy the s2i scripts into the image  
COPY ./s2i/bin $STI_SCRIPTS_PATH  
  
# chown the app directories to the correct user  
RUN chown -R 1001:0 $HOME && \  
chmod -R g+rw $HOME && \  
chmod -R g+rx $STI_SCRIPTS_PATH && \  
chmod g=u /etc/passwd  
  
# switch to the user 1001  
USER 1001  
# show usage info as a default command  
CMD ["$STI_SCRIPTS_PATH/usage"]  

