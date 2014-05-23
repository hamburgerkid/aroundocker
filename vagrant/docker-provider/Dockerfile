FROM centos:6.4
MAINTAINER hamburgerkid

# yum
RUN rpm -Uvh http://dl.fedoraproject.org/pub/epel/6/x86_64/epel-release-6-8.noarch.rpm
RUN yum update -y

# supervisor
RUN yum install -y python-pip && pip install pip --upgrade
RUN pip install supervisor
RUN mkdir -p /var/log/supervisor
ADD supervisord.conf /etc/

# openssh
RUN yum install -y passwd openssh-server
RUN mkdir -p /var/run/sshd
RUN echo 'root:1234' | chpasswd
RUN /usr/bin/ssh-keygen -t rsa -b 2048 -f /etc/ssh/ssh_host_rsa_key -N ""
RUN /usr/bin/ssh-keygen -t dsa -b 1024 -f /etc/ssh/ssh_host_dsa_key -N ""

# apache http server
RUN yum install -y httpd
RUN echo 'Hello, vagrant docker provider! :D' > /var/www/html/index.html
ENV APACHE_RUN_USER  apache
ENV APACHE_RUN_GROUP apache
ENV APACHE_LOG_DIR   /var/log/httpd

# shared volume
RUN mkdir /var/vol
VOLUME ["/var/vol"]

EXPOSE 22 80
CMD ["/usr/bin/supervisord"]
