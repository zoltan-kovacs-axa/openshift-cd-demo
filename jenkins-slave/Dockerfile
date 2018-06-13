# The FROM will be replaced when building in OpenShift
FROM centos:7

MAINTAINER Siamak Sadeghianfar <ssadeghi@redhat.com>

ENV MAVEN_VERSION 3.3.9

# Install headless Java
USER root
RUN yum clean all
ENV INSTALL_PKGS="nss_wrapper java-1.8.0-openjdk-headless \
        java-1.8.0-openjdk-devel nss_wrapper gettext tar git \
        which origin-clients"
RUN yum clean all
RUN yum -y --setopt=tsflags=nodocs install epel-release centos-release-openshift-origin
RUN yum install -y --setopt=tsflags=nodocs install $INSTALL_PKGS
#RUN rpm -V $INSTALL_PKGS
RUN yum clean all
RUN mkdir -p /var/lib/jenkins
RUN chown -R 1001:0 /var/lib/jenkins
RUN chmod -R g+w /var/lib/jenkins

# Install Maven
RUN curl -sL -o /tmp/maven.tar.gz \
      https://repo.maven.apache.org/maven2/org/apache/maven/apache-maven/${MAVEN_VERSION}/apache-maven-${MAVEN_VERSION}-bin.tar.gz
RUN mkdir -p /var/lib/jenkins/tools
RUN cd /var/lib/jenkins/tools
RUN tar xfz /tmp/maven.tar.gz
RUN ln -s /var/lib/jenkins/tools/apache-maven-${MAVEN_VERSION} /var/lib/jenkins/tools/maven
RUN chown -R 1001:0 /var/lib/jenkins/tools

# Copy the entrypoint
COPY configuration/* /var/lib/jenkins/
USER 1001

# Run the JNLP client by default
# To use swarm client, specify "/var/lib/jenkins/run-swarm-client" as Command
ENTRYPOINT ["/var/lib/jenkins/run-jnlp-client"]
