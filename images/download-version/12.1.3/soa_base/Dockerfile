# LICENSE CDDL 1.0 + GPL 2.0
#
# ORACLE DOCKERFILES PROJECT
# --------------------------
# This is the Dockerfile for Oracel SOA Suite 12c (Full)
# 
# IMPORTANT
# ---------
# It's necessary the copy a valid cookies file from your Web browser to download Oracle Software from https://edelivery.oracle.com 
#
# REQUIRED FILES TO BUILD THIS IMAGE
# ----------------------------------
# It's necessary the copy a valid cookies file from your Web browser to download Oracle Software from https://edelivery.oracle.com 
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
#      $ sudo docker build -t fmw12c/soa:12.1.3 . 
#
# From WebLogic 12c base image
# ---------------
FROM fmw12c/wls:12.1.3

# Maintainer
# ----------
MAINTAINER Jacques Lindemann <jacques.lindemann@oracle.com>

# Environment variable required with a proxy
ENV http_proxy=http://emea-proxy.uk.oracle.com:80 https_proxy=http://emea-proxy.uk.oracle.com:80

# Environment variables required for this build

ENV INFRA_PKG=fmw_12.1.3.0.0_infrastructure OSB_PKG=fmw_12.1.3.0.0_osb SOA_PKG=fmw_12.1.3.0.0_soa MFT_PKG=fmw_12.1.3.0.0_mft JAVA_HOME=/usr/java/default

# Setup required packages (unzip), filesystem, and oracle user
# ------------------------------------------------------------


# Copy packages
COPY cookies.txt /u01/
COPY infra.rsp /u01/
COPY soa.rsp /u01/
COPY osb.rsp /u01/
COPY mft.rsp /u01/



# Adjust file permissions, go to /u01 as user 'oracle' to proceed with WLS installation
WORKDIR /u01
USER oracle

# Download Infrastructure
RUN curl --silent --location --retry 3 --cookie cookies.txt "https://edelivery.oracle.com/EPD/Download/process_download/V44416-01.zip?file_id=71652843&aru=17485597&userid=1815772&egroup_aru_number=15364655&country_id=250&patch_file=V44416-01.zip" -o $INFRA_PKG.zip
RUN unzip $INFRA_PKG.zip && rm $INFRA_PKG.zip

# Installation of Infrastructure 
COPY oraInst.loc /u01/
RUN java -jar $INFRA_PKG.jar -ignoreSysPrereqs -novalidation -silent -responseFile /u01/infra.rsp -invPtrLoc /u01/oraInst.loc -jreLoc $JAVA_HOME && \
    rm $INFRA_PKG.jar oraInst.loc infra.rsp



# Download SOA
RUN curl --silent --location --retry 3 --cookie cookies.txt "https://edelivery.oracle.com/EPD/Download/process_download/V44420-01.zip?file_id=71609117&aru=17485841&userid=1815772&egroup_aru_number=15364655&country_id=250&patch_file=V44420-01.zip" -o $SOA_PKG.zip
RUN unzip $SOA_PKG.zip && rm $SOA_PKG.zip

# Installation of SOA 
COPY oraInst.loc /u01/
RUN java -jar $SOA_PKG.jar -ignoreSysPrereqs -novalidation -silent -responseFile /u01/soa.rsp -invPtrLoc /u01/oraInst.loc -jreLoc $JAVA_HOME && \
    rm $SOA_PKG.jar oraInst.loc soa.rsp



# Download OSB
RUN curl --silent --location --retry 3 --cookie cookies.txt "https://edelivery.oracle.com/EPD/Download/process_download/V44423-01.zip?file_id=71612803&aru=17485844&userid=1815772&egroup_aru_number=15364655&country_id=250&patch_file=V44423-01.zip" -o $OSB_PKG.zip
RUN unzip $OSB_PKG.zip && rm $OSB_PKG.zip

# Installation of OSB 
COPY oraInst.loc /u01/
RUN java -jar $OSB_PKG.jar -ignoreSysPrereqs -novalidation -silent -responseFile /u01/osb.rsp -invPtrLoc /u01/oraInst.loc -jreLoc $JAVA_HOME && \
    rm $OSB_PKG.jar oraInst.loc osb.rsp



# Download MFT
RUN curl --silent --location --retry 3 --cookie cookies.txt "https://edelivery.oracle.com/EPD/Download/process_download/V44424-01.zip?file_id=71612527&aru=17485845&userid=1815772&egroup_aru_number=15364655&country_id=250&patch_file=V44424-01.zip" -o $MFT_PKG.zip
RUN unzip $MFT_PKG.zip && rm $MFT_PKG.zip

# Installation of MFT 
COPY oraInst.loc /u01/
RUN java -jar $MFT_PKG.jar -ignoreSysPrereqs -novalidation -silent -responseFile /u01/mft.rsp -invPtrLoc /u01/oraInst.loc -jreLoc $JAVA_HOME && \
    rm $MFT_PKG.jar oraInst.loc mft.rsp cookies.txt


VOLUME /u01/oracle/
WORKDIR /u01/oracle/

#ENV PATH $PATH:/u01/oracle/weblogic/oracle_common/common/bin

# Define default command to start bash. 
CMD ["bash"]

