
#
#  START OS SETUP ##################
#

FROM ubuntu:trusty


# Use "apt-get" "mirror://" method to use a fastest/closes mirror
#RUN sed -i 's/http:\/\/archive.ubuntu.com\/ubuntu\//mirror:\/\/mirrors.ubuntu.com\/mirrors.txt/' /etc/apt/sources.list

# Trigger "apt-get" database refresh by incrementing
ENV APT_REFRESH 08082016

# Update de "apt-get" database
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq update

# Install/update useful packages
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq -y --no-install-recommends install \
    bash \
    binutils \
    git \
    locales \
    inetutils-ping \
    curl \
    supervisor \
    ca-certificates \
    dnsutils

# Generate, set and configure locales to en_US.UTF-8
RUN locale-gen en_US.UTF-8

ENV LANGUAGE=en_US.UTF-8
ENV LANG=en_US.UTF-8
ENV LC_ALL=en_US.UTF-8

RUN dpkg-reconfigure -f noninteractive locales

# Clean the "apt-get" package cache and the "apt-get" lists cache
RUN DEBIAN_FRONTEND=noninteractive apt-get -qq clean; rm -rf /var/lib/apt/lists/*
#
#  END OS SETUP ##################
#

#
#  START APP SETUP ##################
#


RUN curl -L https://github.com/mailgun/kafka-pixy/releases/download/v0.13.0/kafka-pixy-v0.13.0-linux-amd64.tar.gz | tar xz

WORKDIR kafka-pixy-v0.13.0-linux-amd64
COPY default.yaml config.yaml

CMD ./kafka-pixy --config config.yaml
#
#  END APP SETUP ##################
#
