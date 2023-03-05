FROM debian:buster

EXPOSE 11740

ARG CDS_VERSION=4.7.0.0
ARG EDGE_VERSION=4.7.0.0

RUN apt-get update -y; apt-get install -y wget unzip libusb-1.0-0 lsb-base procps \
    multiarch-support libfontconfig1 libfreetype6 libice6 libsm6 libx11-6 libxcb1 \
    socat cron

# set user to root
USER root
ENV USER=root

# install runtime
RUN wget --output-document=install.sh https://forge.codesys.com/trg/linux-sl/install; \
    bash ./install.sh ${CDS_VERSION} ${EDGE_VERSION}

# Add Restart crontab script to restart Codesys Runtime
ADD ./Codesys-Runtime-Linux-SL/crontab /etc/cron.d/codesys-crontab
RUN chmod 0644 /etc/cron.d/codesys-crontab
RUN crontab /etc/cron.d/codesys-crontab
RUN service cron start

# define startup behavior
ADD ./Codesys-Runtime-Linux-SL/startup.sh /
RUN chmod +x startup.sh
WORKDIR /var/opt/codesys/
CMD [ "/startup.sh" ]

