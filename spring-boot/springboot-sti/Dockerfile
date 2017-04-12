# springboot-sti
FROM rhel7.2
MAINTAINER Tarun Mehra <tmehra@redhat.com>

# Install build tools on top of base image
# Java jdk 8, Maven 3.3, Gradle 2.6
ENV MAVEN_VERSION 3.3.9

# Install build tools on top of base image
# Java jdk 8, Maven 3.3, Gradle 2.6

RUN yum-config-manager --enable rhel-server-rhscl-7-rpms && \    
    yum-config-manager --disable epel >/dev/null || : && \
    INSTALL_PKGS="java-1.8.0-openjdk java-1.8.0-openjdk-devel rh-maven33*" && \
    yum install -y $INSTALL_PKGS && \
    rpm -V $INSTALL_PKGS && \
    yum clean all -y && \
    mkdir -p /opt/openshift && \
    mkdir -p /opt/app-root/source && chmod -R a+rwX /opt/app-root/source && \
    mkdir -p /opt/s2i/destination && chmod -R a+rwX /opt/s2i/destination && \
    mkdir -p /opt/app-root/src && chmod -R a+rwX /opt/app-root/src && \
    mkdir -p $HOME/.m2 && chmod -R a+rwX $HOME/.m2

ENV PATH=/opt/maven/bin/:$PATH


ENV BUILDER_VERSION 1.0

LABEL io.k8s.description="Platform for building Spring Boot applications with maven or gradle" \
      io.k8s.display-name="Spring Boot builder 1.0" \
      io.openshift.expose-services="8080:http" \
      io.openshift.tags="builder,maven-3,gradle-2.6,springboot"

# TODO (optional): Copy the builder files into /opt/openshift
# COPY ./<builder_folder>/ /opt/openshift/
# COPY Additional files,configurations that we want to ship by default, like a default setting.xml

LABEL io.openshift.s2i.scripts-url=image:///usr/local/sti
COPY ./.sti/bin/ /usr/local/sti

RUN chown -R 1001:1001 /opt/openshift

# This default user is created in the openshift/base-centos7 image
USER 1001

# Set the default port for applications built using this image
EXPOSE 8080

# Set the default CMD for the image
# CMD ["java","-Djava.security.egd=file:/dev/./urandom","-jar","/opt/openshift/app.jar"]
CMD ["usage"]
