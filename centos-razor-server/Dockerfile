# Create a storage container before running this
# docker run -v /var/lib/razor/repo-store -v /var/lib/pgsql --name razor-repo tianon/true

FROM tianon/centos:6.5

RUN  echo export http_proxy=http://`netstat -nr | grep '^0\.0\.0\.0' | awk '{print $2}'`:3128 >> /etc/profile; \
     echo export https_proxy=http://`netstat -nr | grep '^0\.0\.0\.0' | awk '{print $2}'`:3128 >> /etc/profile; \
     echo export ftp_proxy=http://`netstat -nr | grep '^0\.0\.0\.0' | awk '{print $2}'`:3128 >> /etc/profile; \
     echo proxy=http://`netstat -nr | grep '^0\.0\.0\.0' | awk '{print $2}'`:3128 >> /etc/yum.conf

RUN  yum -y update

RUN  rpm -ivh https://yum.puppetlabs.com/el/6/products/x86_64/puppetlabs-release-6-10.noarch.rpm && \
     yum install -y puppet tar

# Per Beaker docs, these are requirements
# https://github.com/puppetlabs/beaker/wiki/Creating-A-Test-Environment
# RUN yum install -y curl ntp git ruby

ADD  razor-postgres-install.pp /
ADD  start.sh /

ENV  TORQUEBOX_HOME /opt/razor-torquebox
ENV  JBOSS_HOME     $TORQUEBOX_HOME/jboss
ENV  JRUBY_HOME     $TORQUEBOX_HOME/jruby
ENV  PATH           $JRUBY_HOME/bin:$PATH

RUN  puppet module install puppetlabs/postgresql
RUN  puppet module install puppetlabs/razor
RUN  puppet apply -e 'include razor'

#VOLUME    /var/lib/razor/repo-store
#VOLUME    /var/lib/pgsql

# 69      TFTP
# 8080	Torquebox API
EXPOSE    69 8080

RUN  chmod +x /start.sh
CMD  ["/start.sh"]
