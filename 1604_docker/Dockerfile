FROM phusion/baseimage:latest
MAINTAINER skysider <skysider@163.com>

#RUN mv /etc/apt/sources.list /etc/apt/sources.list.bak \
#	touch /etc/apt/sources.list

RUN echo 'deb-src http://archive.ubuntu.com/ubuntu xenial main restricted'> /etc/apt/sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted'>> /etc/apt/sources.list 
RUN echo 'deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multiverse universe'>> /etc/apt/sources.list 
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted'>> /etc/apt/sources.list  
RUN echo 'deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe'>> /etc/apt/sources.list 
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial universe'>> /etc/apt/sources.list 
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe'>> /etc/apt/sources.list
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse'>> /etc/apt/sources.list 
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse'>> /etc/apt/sources.list 
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse'>> /etc/apt/sources.list 
RUN echo 'deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse'>> /etc/apt/sources.list  
RUN echo 'deb http://archive.canonical.com/ubuntu xenial partner'>> /etc/apt/sources.list 
RUN echo 'deb-src http://archive.canonical.com/ubuntu xenial partner'>> /etc/apt/sources.list 
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted'>> /etc/apt/sources.list 
RUN echo 'deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe'>> /etc/apt/sources.list 
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe'>> /etc/apt/sources.list  
RUN echo 'deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse'>> /etc/apt/sources.list


RUN dpkg --add-architecture i386 && \
    apt-get -y update && \
    apt install -y \
    libc6:i386 \
    libc6-dbg:i386 \
    libc6-dbg \
    lib32stdc++6 \
    g++-multilib \
    cmake \
    net-tools \
    libffi-dev \
    libssl-dev \
    python3-pip \
    python-pip \
    python-capstone \
    ruby2.3 \
    tmux \
    strace \
    ltrace \
    nasm \
    wget \
    radare2 \
    gdb \
    netcat \
    socat \
    git \
    patchelf \
    gawk \
    file --fix-missing && \
    rm -rf /var/lib/apt/list/*

RUN mkdir ~/.pip
RUN touch ~/.pip/pip.conf

RUN echo '[global]' > ~/.pip/pip.conf
RUN echo 'timeout = 6000' >> ~/.pip/pip.conf
RUN echo 'index-url = https://pypi.mirrors.ustc.edu.cn/simple/' >> ~/.pip/pip.conf
RUN echo 'trusted-host = pypi.mirrors.ustc.edu.cn' >> ~/.pip/pip.conf

WORKDIR /ctf/work

RUN pip3 install --no-cache-dir \
    ropper \
    unicorn \
    keystone-engine \
    capstone

#pip install --upgrade pip\

RUN pip install --upgrade setuptools && \
    pip install --no-cache-dir \
    ropgadget \
    pwntools \
    zio \
    angr \
    lief \
    z3-solver \
    apscheduler && \
    pip install --upgrade pwntools

RUN gem install \
    one_gadget && \
    rm -rf /var/lib/gems/2.3.*/cache/*

RUN git clone https://github.com/pwndbg/pwndbg && \
    cd pwndbg && sed -i s/sudo//g setup.sh && \
    chmod +x setup.sh && ./setup.sh

RUN git clone https://github.com/skysider/LibcSearcher.git LibcSearcher && \
    cd LibcSearcher && git submodule update --init --recursive && \
    python setup.py develop && cd libc-database && ./get || ls
WORKDIR /ctf/work/

RUN cd /ctf && mkdir glibc && cd glibc && mkdir 2.24 && cd /ctf/work && \
    wget http://mirrors.ustc.edu.cn/gnu/libc/glibc-2.24.tar.gz && \
    tar xf glibc-2.24.tar.gz && cd glibc-2.24 && mkdir build && cd build && \
    ../configure --prefix=/ctf/glibc/2.24/ --disable-werror --enable-debug=yes && \
    make && make install && cd ../../ && rm -rf glibc-2.24 && rm glibc-2.24.tar.gz

COPY linux_server linux_server64 /ctf/

RUN chmod a+x /ctf/linux_server /ctf/linux_server64


ENTRYPOINT ["/bin/bash"]
