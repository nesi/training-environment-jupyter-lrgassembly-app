FROM ubuntu:22.04

# run unminimise, e.g. for man pages to work
RUN yes | unminimize

# install system dependencies
# TODO: move ldap-utils, libnss-ldapd, libpam-ldapd, nscd, nslcd to base image??
RUN echo 'tzdata tzdata/Areas select Pacific' | debconf-set-selections \
    && echo 'tzdata tzdata/Zones/Pacific select Auckland' | debconf-set-selections \
    && apt-get update && DEBIAN_FRONTEND=noninteractive apt-get install -y \
        bioawk \
        curl \
        fakeroot \
        file \
        git \
        ldap-utils \
        libnss-ldapd \
        libpam-ldapd \
        less \
        locales \
        man-db \
        nano \
        nodejs \
        nscd \
        nslcd \
        python-is-python3 \
        python3 \
        python3-pip \
        rsync \
        software-properties-common \
        tzdata \
        unzip \
        vim \
        wget \
        zip \
        openjdk-17-jdk openjdk-17-jre \
    && rm -rf /var/lib/apt/lists/* \
    && localedef -i en_NZ -c -f UTF-8 -A /usr/share/locale/locale.alias en_NZ.UTF-8


ENV LANG en_NZ.utf8

# install apptainer
RUN add-apt-repository -y ppa:apptainer/ppa \
    && apt update \
    && apt install -y apptainer-suid \
    && rm -rf /var/lib/apt/lists/*

# install applications via conda
RUN wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh -O /miniconda.sh \
    && bash /miniconda.sh -b -p /opt/miniconda3 \
    && rm -f /miniconda.sh \
    && . /opt/miniconda3/etc/profile.d/conda.sh \
    && conda config --set auto_activate_base false \
    && conda config --add channels defaults \
    && conda config --add channels bioconda \
    && conda config --add channels conda-forge \
    && conda config --set channel_priority strict \
    && conda create --yes --prefix /var/lib/genome-assembly \
       python=3.10 \
       pigz \
       fastqc \
       multiqc \
       samtools \
       bamtools \
       nanocomp \
       cutadapt \
       seqkit \
       merqury \
       meryl \
       verkko \
       yak \
       hifiasm \
       gfastats \
       minimap2 \
       liftoff \
       mashmap \
    && echo ". /opt/miniconda3/etc/profile.d/conda.sh" > /etc/profile.d/conda.sh \
    && echo "conda activate /var/lib/genome-assembly" >> /etc/profile.d/conda.sh


# install jupyterlab 
RUN pip3 --no-cache-dir install jupyterlab
