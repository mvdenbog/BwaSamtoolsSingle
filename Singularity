Bootstrap: docker
From: centos:7.4.1708


%help

    This is a container for executing SnapAligner and 2 python3 scripts to plot the results.


%setup

    # make sure the "/pipeline/scripts" folder exists
    # mkdir -p ${SINGULARITY_ROOTFS}/pipeline/scripts
    # make sure the "/data" folder exists
    # mkdir ${SINGULARITY_ROOTFS}/data
    mkdir ${SINGULARITY_ROOTFS}/pasteur

%post

    # Enable repo
    yum -y install epel-release
    yum-config-manager --enable epel

    # Enable repo for installing python36 where you can "easily" install tkinter (we need it for the plotting python scripts, after doing snap):

    # Install gcc compiler and zlib (we need it to compile snap, htslib, samtools):
    yum -y install gcc
    yum -y install zlib zlib-devel
    yum -y install wget bzip2  ncurses ncurses-devel

    # Install Python (3.6.5), pip and Snakemake
     yum -y install https://centos7.iuscommunity.org/ius-release.rpm

    # install snap (we need g++ too):
    yum -y install git make gcc-c++
    yum -y install python python-pip python-devel
    pip install --upgrade pip
    pip install matplotlib

    # install samtools 0.6.2
   cd /opt/
   wget -O samtools-0.1.19.tar.bz2 http://sourceforge.net/projects/samtools/files/samtools/0.1.19/samtools-0.1.19.tar.bz2/download
   tar xjf samtools-0.1.19.tar.bz2
   cd samtools-0.1.19
   make
   chown -R root:root /opt/samtools-0.1.19
   chmod -R a+rx /opt/samtools-0.1.19
   cd /opt/
   wget http://downloads.sourceforge.net/project/bio-bwa/bwa-0.6.2.tar.bz2
   tar xjf bwa-0.6.2.tar.bz2
   cd bwa-0.6.2
   make
   chown -R root:root /opt/bwa-0.6.2
   chmod -R a+rx /opt/bwa-0.6.2
   echo 'export PATH=$PATH:/opt/samtools-0.1.19/:/opt/samtools-0.1.19/bcftools/:/opt/bwa-0.6.2/' >> $SINGULARITY_ENVIRONMENT
   
%files

    # Copy pipeline scripts
    bwa_samtools_se.sh /opt/
    depth_of_coverage_v2.py /opt/


%runscript

    sh /opt/bwa_samtools_se.sh "$@"


%labels

    MAINTAINER Mathias Vandenbogaert
    VERSION 01


%test

