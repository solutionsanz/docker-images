# LICENSE UPL 1.0
#
# Copyright (c) 2014-2018 Oracle and/or its affiliates. All rights reserved.
#
# ORACLE DOCKERFILES PROJECT
# --------------------------
#
# Dockerfile template for Oracle Instant Client
#
# REQUIRED FILES TO BUILD THIS IMAGE
# ==================================
# 
# From http://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html
#  Download the following three RPMs:
#    - oracle-instantclient18.3-basic-18.3.0.0.0-1.x86_64.rpm
#    - oracle-instantclient18.3-devel-18.3.0.0.0-1.x86_64.rpm
#    - oracle-instantclient18.3-sqlplus-18.3.0.0.0-1.x86_64.rpm 
#
# HOW TO BUILD THIS IMAGE
# -----------------------
# Put all downloaded files in the same directory as this Dockerfile
# Run: 
#      $ docker build -t oracle/instantclient-adw-py:18.3.0 . 
#
#      $ docker run -it --rm oracle/instantclient-adw-py:18.3.0 bash
#
#
FROM oraclelinux:7-slim

ADD oracle-instantclient*.rpm /tmp/

RUN  yum -y install /tmp/oracle-instantclient*.rpm && \
     rm -rf /var/cache/yum && \
     rm -f /tmp/oracle-instantclient*.rpm && \
     echo /usr/lib/oracle/18.3/client64/lib > /etc/ld.so.conf.d/oracle-instantclient18.3.conf && \
     ldconfig

ENV PATH=$PATH:/usr/lib/oracle/18.3/client64/bin
ENV LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/oracle/18.3/client64/lib

RUN yum -y install yum-utils && \
    yum-config-manager --enable ol7_developer_EPEL && \
    yum-config-manager --enable ol7_developer

RUN mkdir -p /home/pythonscripts && \
    mkdir -p /home/adw_wallet

ADD ./*.py /home/pythonscripts/ 
ADD ./adw_wallet/* /home/adw_wallet/

ENV TNS_ADMIN=/home/adw_wallet

RUN yum install -y gcc python-devel 
RUN yum install -y python-pip 
RUN pip install --upgrade pip 
#RUN yum -y install --skip python-cx_Oracle 
RUN python -m pip install cx_Oracle --upgrade
RUN pip install cx_Oracle

CMD ["sqlplus", "-v"]

