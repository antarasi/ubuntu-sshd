FROM       ubuntu:16.04
MAINTAINER Adrian Matylewicz "https://github.com/antarasi"

ARG DEBIAN_FRONTEND=noninteractive

RUN apt-get update
RUN apt-get install -y openssh-server
RUN mkdir /var/run/sshd

RUN useradd -ms /bin/bash ubuntu \
    && echo 'root:root' | chpasswd \
    && echo 'ubuntu:ubuntu' | chpasswd \
    # SSH login fix. Otherwise user is kicked off after login
    && sed 's@session\s*required\s*pam_loginuid.so@session optional pam_loginuid.so@g' -i /etc/pam.d/sshd

RUN mkdir /root/.ssh

RUN apt-get clean && \
    rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

EXPOSE 22

CMD    ["/usr/sbin/sshd", "-D"]
