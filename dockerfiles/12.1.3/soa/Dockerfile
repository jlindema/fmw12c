# LICENSE CDDL 1.0 + GPL 2.0
#
# ORACLE DOCKERFILES PROJECT
# --------------------------
# This is the Dockerfile for Oracel SOA Suite 12c (Full)
# 
# IMPORTANT
# ---------
# It's necessary the copy a valid cookies file from your Web browser to download Oracle Software from http://www.oracle.com 
#
# REQUIRED FILES TO BUILD THIS IMAGE
# ----------------------------------
# It's necessary the copy a valid cookies file from your Web browser to download Oracle Software from http://www.oracle.com 
# If you don’t already have a tool to export the cookies from the Web browser to a text file, I’d suggest one of the browser extensions below:
#
#    For Firefox: "Export Cookies" (https://addons.mozilla.org/en-US/firefox/addon/export-cookies/?src=api) 
#    For Chrome: "cookies.txt export" (https://chrome.google.com/webstore/detail/cookietxt-export/lopabhfecdfhgogdbojmaicoicjekelh)
#
# After installing the extension(s) above on the browser of your choice, follow the steps below:
#
#    Initiate the download of the file you want (if downloading multiple files, you just need to do this for the first one)
#    Once the download is initiated, cancel it.
#    Export the cookies to a file (call it cookies.txt)
#    If you’re using one of the extensions suggested above, this is how you do it:
#
#        On Firefox: click on Tools -> “Export cookies…” and save the file
#        On Chrome: click on the “cookies.txt export” icon in the toolbar (the icon is a blue “C” with an arrow inside), select the entire contents of the cookies and paste it into #	 a text file.
#    Copy the cookies.txt file in the same directory as this Dockerfile.
#
#
# HOW TO BUILD THIS IMAGE
# -----------------------
# Put the cookies.txt file in the same directory as this Dockerfile
# Run: 
#      $ sudo docker build -t fmw/soa:12.1.3 . 
#
# Pull base image
# ---------------
FROM fmw/wls:12.1.3

# Maintainer
# ----------
MAINTAINER Jacques Lindemann <jacques.lindemann@oracle.com>

# Environment variable required with a proxy
# ENV http_proxy http://emea-proxy.uk.oracle.com:80
# ENV https_proxy http://emea-proxy.uk.oracle.com:80
# ENV HTTP_PROXY http://emea-proxy.uk.oracle.com:80
# ENV HTTPS_PROXY http://emea-proxy.uk.oracle.com:80

# Environment variables required for this build

ENV JAVA_VERSION 7
ENV JAVA_UPDATE 75
ENV JAVA_BUILD 13

ENV WLS_PKG fmw_12.1.3.0.0_wls.jar
ENV JAVA_HOME /usr/java/default

# Setup required packages (unzip), filesystem, and oracle user
# ------------------------------------------------------------
RUN mkdir /u01 && \
    chmod a+xr /u01 && \
    useradd -b /u01 -m -s /bin/bash oracle 

# Copy packages
COPY cookies.txt /u01/

RUN curl --silent --location --retry 3 --cookie /u01/cookies.txt --insecure http://download.oracle.com/otn-pub/java/jdk/"${JAVA_VERSION}"u"${JAVA_UPDATE}"-b"${JAVA_BUILD}"/jdk-"${JAVA_VERSION}"u"${JAVA_UPDATE}"-linux-x64.rpm -o /u01/jdk-"${JAVA_VERSION}"u"${JAVA_UPDATE}"-linux-x64.rpm

# COPY $WLS_PKG /u01/
RUN curl --silent --location --retry 3 --cookie /u01/cookies.txt --insecure http://download.oracle.com/otn/nt/middleware/12c/wls/1213/$WLS_PKG -o /u01/$WLS_PKG

COPY wls.rsp /u01/
COPY infra.rsp /u01/
COPY osb.rsp /u01/
COPY soa.rsp /u01/
COPY bpm.rsp /u01/
COPY mft.rsp /u01/
COPY oraInst.loc /u01/

# Install and configure Oracle JDK
# --------------------------------
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
WORKDIR /u01
USER oracle

# Installation of WebLogic 
RUN mkdir /u01/oracle/.inventory
RUN java -jar $WLS_PKG -ignoreSysPrereqs -novalidation -silent -responseFile /u01/install.file -invPtrLoc /u01/oraInst.loc -jreLoc $JAVA_HOME && \
    rm $WLS_PKG /u01/oraInst.loc /u01/install.file

WORKDIR /u01/oracle/

#ENV PATH $PATH:/u01/oracle/weblogic/oracle_common/common/bin

# Define default command to start bash. 
CMD ["bash"]

