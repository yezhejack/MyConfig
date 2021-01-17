FROM nvidia/cuda:10.2-devel-ubuntu18.04

#  $ docker build . -t continuumio/anaconda3:latest -t continuumio/anaconda3:5.3.0
#  $ docker run --rm -it continuumio/anaconda3:latest /bin/bash
#  $ docker push continuumio/anaconda3:latest
#  $ docker push continuumio/anaconda3:5.3.0

#ENV LANG="en_US.UTF-8"

RUN apt-get update --fix-missing && apt-get install -y wget bzip2 ca-certificates \
    libglib2.0-0 libxext6 libsm6 libxrender1 \
    git mercurial subversion sudo openssh-server htop vim locales sed screen && apt-get clean

# Setup ssh
#RUN apt-get update && apt-get install -y openssh-server htop vim locales && apt-get clean

#RUN service ssh start
#RUN apt-get update && apt-get install -y sed && apt-get clean
RUN sed -i 's/#PermitRootLogin prohibit-password/PermitRootLogin yes/' /etc/ssh/sshd_config
RUN sed -i 's/#PubkeyAuthentication yes/PubkeyAuthentication yes/' /etc/ssh/sshd_config
RUN cd ~ && mkdir .ssh
RUN echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQDR9srMYUUFaY6K5caY5cSMajjSOgLy2ZQP13dGRlS1SU8AV2Hmi7PR7BB22IPI650pY7QA2ExsBawnsAbm3SD5V2Yez6zqwr2P+APaOu/Kq2TaPGW4zbi0t92FGhzXWz8G9NuuSKv8lB0n52ldK4ZaPWTnP9jiuCi5XpHoVBjaD5MwU7U5XfZTtDDZub+79uk4oqAqtHvl+kjaRjCAw2qTKB0lI+jDcBWPWxhS7mrz1o9dRqHBQHQ5DvUiQjBkSrxcBJUD+5PN9J7MisdOzqNaiWBBEneLLo+kMr9nu35T3bGEmk3VyzKnw8rHATzRNnW3LB7LqJjcBCZVkg0bJ6CN fareast\zheye@MININT-OMO17JN' >> ~/.ssh/authorized_keys
RUN echo 'ssh-rsa AAAAB3NzaC1yc2EAAAADAQABAAABAQCoYSskw5pVwBcG6hULJTlUCHciXKYPTcxcNZTxuQ3q17r8KkRAOZekgVN3+EIJ/mvWKNQQjbuKVaxDllFEMXgfMWHCZXX2vQHUQdVrpqaDF5gcEqxNoLzDAD2PqUnGldSTc9K/7Y0V8rdMpgXanfpxzx1W8HLhmm858Z8P+odTB4kr+RQkTgOUkOCTxIsx5ZTmWj6QWz3o5FjkaTRxEekjE9jSfXVW9aHARv+yuQGOzBv9tq97389j4V3D/DP25BhQRaOw1Wga+K/WpivpoCDnYWmA0DTniwVFSAATQ1rBAtbf77E597M3EDvygZJGt+gIkR9Jpki+4QTM4xZM0axH yezhejack@gmail.com' >> ~/.ssh/authorized_keys
#RUN /bin/bash service ssh start
RUN echo '/bin/bash service ssh start && /bin/bash' > /root/init.sh
CMD ["/bin/bash", "/root/init.sh"]

# Setup locale
RUN locale-gen en_US.UTF-8
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
RUN echo "export LANG=en_US.UTF-8" >> ~/.bashrc && echo "export LANGUAGE=en_US.UTF-8" >> ~/.bashrc && echo "export LC_ALL=en_US.UTF-8" >> ~/.bashrc

# Setup User
RUN useradd -u 1011 -ms /bin/bash zheye
RUN usermod -aG sudo zheye
RUN mkdir /home/zheye/.ssh
RUN cp /root/.ssh/authorized_keys /home/zheye/.ssh/authorized_keys
RUN chown -R zheye /home/zheye/.ssh/
USER zheye
ENV PATH /home/zheye/anaconda3/bin:$PATH
ENV LANG en_US.UTF-8
ENV LANGUAGE en_US:en
ENV LC_ALL en_US.UTF-8
RUN wget --quiet https://github.com/yezhejack/MyConfig/raw/master/screenrc -O ~/.screenrc

RUN wget --quiet https://repo.anaconda.com/archive/Anaconda3-2020.11-Linux-x86_64.sh -O ~/anaconda.sh && \
    /bin/bash ~/anaconda.sh -b -p /home/zheye/anaconda3 && \
    rm ~/anaconda.sh && \
    echo ". /home/zheye/anaconda3/etc/profile.d/conda.sh" >> ~/.bashrc && \
    echo "conda activate base" >> ~/.bashrc

RUN conda install pytorch torchvision cudatoolkit=10.2 -c pytorch && conda clean -y -a
RUN pip install progressbar2

RUN echo "export LANG=en_US.UTF-8" >> ~/.bashrc && echo "export LANGUAGE=en_US.UTF-8" >> ~/.bashrc && echo "export LC_ALL=en_US.UTF-8" >> ~/.bashrc
USER root
WORKDIR /root
