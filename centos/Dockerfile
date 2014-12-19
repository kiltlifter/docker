##################################################
# Docker File to created a dev centos container
##################################################

# Use the base centos6 image
FROM centos:centos6

# Author/Maintainer
MAINTAINER Sean Douglas <seancdouglas@gmail.com>

################## Begin Install ################# 

# Enable the Epel repo
RUN /usr/bin/yum install -y http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm

# Update the system after Epel repo is added
RUN /usr/bin/yum update -y

# Install postgres and supporting services
RUN /usr/bin/yum install -y postgresql postgresql-libs postgresql-devel postgresql-server postgresql-contrib

# Initialize the postgres database
RUN /sbin/service postgresql initdb

# Install openjdk
RUN /usr/bin/yum -y install java-1.7.0-openjdk java-1.7.0-openjdk-devel

# Add useful tools for rpm creation
RUN /usr/bin/yum -y install rpm-build rpmdevtools mock redhat-rpm-config createrepo

# Add some security tools
RUN /usr/bin/yum -y install setools libsemanage libsemanage-devel setools-console setools-devel setools-devel setools-libs policycoreutils-python nmap libpcap-devel setroubleshoot

# Add some misc. dev tools
RUN /usr/bin/yum -y install git p7zip p7zip-plugins gcc-c++ zlib-devel httpd httpd-devel apr-devel apr-util-devel libyaml ant subversion openssh openssh-server unzip

# Add ruby
#RUN /usr/bin/curl -sSL "http://get.rvm.io" --output /tmp/rvm-install.sh

# Run the ruby installer
#RUN /bin/bash /tmp/rvm-install.sh --ruby=2.0.0

# sshd service configuration
# source: https://docs.docker.com/examples/running_ssh_service/
RUN mkdir /var/run/sshd
RUN echo 'root:docker' | chpasswd
RUN sed -i 's/PermitRootLogin without-password/PermitRootLogin yes/' /etc/ssh/sshd_config
# SSH login fix. Otherwise user is kicked off after login
RUN sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd
ENV NOTVISIBLE "in users profile"
RUN echo "export VISIBLE=now" >> /etc/profile
RUN /etc/init.d/sshd start

# Install apache maven
RUN /usr/bin/curl -G "http://apache.spinellicreations.com/maven/maven-3/3.2.3/binaries/apache-maven-3.2.3-bin.zip" -o /tmp/apache-maven-3.2.3-bin.zip && /usr/bin/unzip /tmp/apache-maven-3.2.3-bin.zip -d /usr/lib/ 

# Added an environment variable for maven
RUN /bin/echo -e "MAVEN_HOME=/usr/lib/apache-maven-3.2.3\nexport PATH=\$PATH:\$MAVEN_HOME/bin" >> /etc/skel/.bashrc

# Clean out /tmp
RUN /bin/rm -rf /tmp/*

# Add a docker user and set the password to docker
RUN /usr/sbin/useradd -m docker
RUN /bin/echo "docker:docker" | /usr/sbin/chpasswd

################## End Install ###################

# Expose the default ssh listening port
EXPOSE 22

# Make sure sshd is running
CMD ["/usr/sbin/sshd", "-D"]
