FROM centos:centos7

# yum update
RUN yum -y update

# sysstat install
RUN yum install -y sysstat

# 作業フォルダ作成
RUN cd /home \
mkdir sysstat-log