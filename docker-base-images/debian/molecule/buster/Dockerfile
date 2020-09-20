FROM debian:buster-slim

LABEL maintainer="Stas Stavnichuk"

ENV container docker
ENV LC_ALL C
ENV DEBIAN_FRONTEND noninteractive

RUN echo 'deb http://deb.debian.org/debian buster-backports main' >> /etc/apt/sources.list

RUN apt-get update \
    && apt-get upgrade -y \
    && apt-get install -y systemd \
    && apt-get install -y python3-minimal python3-zipstream \
    && apt-get install -y iproute2 netcat \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

# Make sure systemd doesn't start agettys on tty[1-6].
RUN rm -f /lib/systemd/system/multi-user.target.wants/getty.target

VOLUME [ "/sys/fs/cgroup" ]
CMD ["/lib/systemd/systemd"]
