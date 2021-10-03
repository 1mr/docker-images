FROM debian:bullseye-slim

LABEL maintainer="Stas Stavnichuk"

ENV container docker
ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive
ENV ANSIBLE_USER=ansible SUDO_GROUP=sudo

RUN echo 'deb http://deb.debian.org/debian bullseye-backports main' >> /etc/apt/sources.list
# hadolint ignore=DL3008
RUN apt-get update \
    && apt-get install -y --no-install-recommends \
       sudo systemd systemd-sysv \
       build-essential wget libffi-dev libssl-dev \
       python3-pip python3-dev python3-setuptools python3-wheel python3-apt \
    && update-alternatives --install /usr/bin/python python /usr/bin/python3 10 \
    && update-alternatives --install /usr/bin/pip pip /usr/bin/pip3 10 \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/* \
    && rm -Rf /usr/share/doc && rm -Rf /usr/share/man \
    && apt-get clean

# hadolint ignore=DL3059
RUN rm -f /lib/systemd/system/multi-user.target.wants/* \
    /etc/systemd/system/*.wants/* \
    /lib/systemd/system/local-fs.target.wants/* \
    /lib/systemd/system/sockets.target.wants/*udev* \
    /lib/systemd/system/sockets.target.wants/*initctl* \
    /lib/systemd/system/sysinit.target.wants/systemd-tmpfiles-setup* \
    /lib/systemd/system/systemd-update-utmp*

# Disable requiretty.
RUN sed -i -e 's/^\(Defaults\s*requiretty\)/#--- \1/'  /etc/sudoers

# Create `ansible` user with sudo permissions
RUN set -xe && \
    groupadd -r ${ANSIBLE_USER} && \
    useradd -m -g ${ANSIBLE_USER} ${ANSIBLE_USER} && \
    usermod -aG ${SUDO_GROUP} ${ANSIBLE_USER} && \
    sed -i "/^%${SUDO_GROUP}/s/ALL\$/NOPASSWD:ALL/g" /etc/sudoers

RUN pip3 install ansible cryptography

VOLUME [ "/sys/fs/cgroup", "/tmp", "/run" ]

CMD [ "/lib/systemd/systemd" ]
