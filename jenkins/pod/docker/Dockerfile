FROM centos:centos7

ENV JENKINS_HOME=/var/lib/jenkins
ENV JAVA_HOME=/usr/lib/jvm/java-1.8.0-openjdk
ENV PATH=${JAVA_HOME}/bin:${PATH}

ARG user=jenkins
ARG group=jenkins
ARG uid=1000
ARG gid=1000

ARG jenkins_version=2.263.3

USER root
RUN yum -y update && yum -y upgrade
RUN yum -y install sudo &&\
            git &&\
            epel-release &&\
            redhat-rpm-config &&\
            dnf &&\
            yum-utils &&\
            wget &&\
            java-1.8.0-openjdk-devel &&\
            gcc &&\
            libffi &&\
            libffi-devel &&\
            libcurl-devel &&\
            openssl-devel &&\
            python-devel &&\
            cairo-devel &&\
            libcairo2-dev &&\
            libjpeg-dev &&\
            libgif-dev &&\
            gobject-introspection-devel &&\
            cairo-gobject-devel &&\
            cairo &&\
            python-pip

RUN groupadd -g ${gid} ${group} &&\
    useradd -rm -u ${uid} -g ${group} -s /bin/sh ${user}
RUN echo "${user} ALL=(ALL) NOPASSWD: ALL" > /etc/sudoers.d/${user} &&\
    chmod 440 /etc/sudoers.d/${user} 

RUN yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo 
RUN yum -y install docker-ce docker-ce-cli containerd.io

RUN wget -O /etc/pki/rpm-gpg/NODESOURCE-GPG-SIGNING-KEY-EL https://rpm.nodesource.com/pub/el/NODESOURCE-GPG-SIGNING-KEY-EL &&\
    rpm --import /etc/pki/rpm-gpg/NODESOURCE-GPG-SIGNING-KEY-EL

COPY requirements.txt /opt/jenkins/requirements.txt
WORKDIR /opt/jenkins

RUN python --version
RUN pip install --upgrade "pip < 21.0"
RUN pip install -r requirements.txt

ARG jenkins_package_path=/usr/lib/jenkins-${jenkins_version}.war
RUN wget -O ${jenkins_package_path} https://get.jenkins.io/war-stable/${jenkins_version}/jenkins.war

# USER ${user}
EXPOSE 8080
CMD ["java", "-jar", "/usr/lib/jenkins-2.263.3.war", "-Djenkins.install.runSetupWizard=false"]
# CMD ["java", "-version"]

https://medium.com/hootsuite-engineering/building-docker-images-inside-kubernetes-42c6af855f25
