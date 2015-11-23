# LICENSE CDDL 1.0 + GPL 2.0
#
# ORACLE DOCKERFILES PROJECT
# --------------------------
# This is the Dockerfile for WebLogic 12c (Full) Generic Distribution
# 
# IMPORTANT
# ---------
# It's necessary the copy a valid cookies file from your Web browser to download Oracle Software from http://www.oracle.com 
#
# REQUIRED FILES TO BUILD THIS IMAGE
# ----------------------------------
# download the JDK and WebLogic from edelivery.oracle.com
# jdk-7u55-linux-x64.rpm -> V44959-01.zip
# fmw_12.1.3.0.0_wls.jar -> V44413-01.zip
#
# HOW TO BUILD THIS IMAGE
# -----------------------
# copy V44959-01.zip and V44413-01.zip files in the same directory as this Dockerfile
# Run: 
#      $ sudo docker build -t fmw12c/wls:12.1.3 . 
#
# Pull base image
# ---------------
FROM oraclelinux:7.1

# Maintainer
# ----------
MAINTAINER Jacques Lindemann <jacques.lindemann@oracle.com>

# Environment variable required with a proxy
ENV http_proxy=http://emea-proxy.uk.oracle.com:80 https_proxy=http://emea-proxy.uk.oracle.com:80


# Environment variables required for this build

ENV JAVA_VERSION=7 JAVA_UPDATE=55  JAVA_HOME=/usr/java/default WLS_PKG=fmw_12.1.3.0.0_wls.jar


# Setup required packages (unzip), filesystem, and oracle user
# ------------------------------------------------------------
RUN yum -y install unzip

RUN mkdir /u01 && \
    chmod a+xr /u01 && \
    useradd -b /u01 -m -s /bin/bash oracle 




# Install and configure Oracle JDK
# --------------------------------

# Copy packages
COPY V44959-01.zip  /u01/

WORKDIR /u01

RUN unzip V44959-01.zip && \
   rm V44959-01.zip

RUN rpm -i /u01/jdk-"${JAVA_VERSION}"u"${JAVA_UPDATE}"-linux-x64.rpm && \ 
    rm /u01/jdk-"${JAVA_VERSION}"u"${JAVA_UPDATE}"-linux-x64.rpm
ENV JAVA_HOME /usr/java/default
RUN sed -i -e "s/securerandom.source=file:\/dev\/urandom/securerandom.source=file:\/dev\/.\/urandom/g" "$JAVA_HOME/jre/lib/security/java.security"

# Change the open file limits in /etc/security/limits.conf
RUN sed -i '/.*EOF/d' /etc/security/limits.conf && \
    echo "* soft nofile 16384" >> /etc/security/limits.conf && \ 
    echo "* hard nofile 16384" >> /etc/security/limits.conf && \ 
    echo "# EOF"  >> /etc/security/limits.conf

# Change the kernel parameters that need changing.
RUN echo "net.core.rmem_max=4192608" > /u01/oracle/.sysctl.conf && \
    echo "net.core.wmem_max=4192608" >> /u01/oracle/.sysctl.conf && \ 
    sysctl -e -p /u01/oracle/.sysctl.conf

# Adjust file permissions, go to /u01 as user 'oracle' to proceed with WLS installation
RUN chown oracle:oracle -R /u01





# Installation of WebLogic 
# --------------------------------

USER oracle

# Copy packages
COPY wls.rsp /u01/
COPY oraInst.loc /u01/
COPY V44413-01.zip  /u01/

RUN mkdir /u01/oracle/.inventory
RUN unzip V44413-01.zip && \
   rm V44413-01.zip

RUN java -jar $WLS_PKG -ignoreSysPrereqs -novalidation -silent -responseFile /u01/wls.rsp -invPtrLoc /u01/oraInst.loc -jreLoc $JAVA_HOME && \
    rm $WLS_PKG /u01/oraInst.loc /u01/wls.rsp

WORKDIR /u01/oracle/

#ENV PATH $PATH:/u01/oracle/weblogic/oracle_common/common/bin

# Define default command to start bash. 
USER root
CMD ["bash"]

