FROM alpine:3.5

RUN echo 'hosts: files dns' >> /etc/nsswitch.conf
RUN apk add --no-cache iputils ca-certificates net-snmp-tools && \
    update-ca-certificates

ENV TELEGRAF_VERSION 1.4.0-nfsclient

RUN set -ex && \
    apk add --no-cache --virtual .build-deps wget gnupg tar && \
    wget -q http://dl.skinfra.cloud/monitoring/telegraf/telegraf-${TELEGRAF_VERSION}-static_linux_amd64 && \
    mv telegraf-${TELEGRAF_VERSION}-static_linux_amd64 telegraf && \
    chmod +x telegraf && \
    mkdir -p /etc/telegraf && \
    cp -a telegraf /usr/bin/ && \
    /usr/bin/telegraf config > /etc/telegraf/telegraf.conf && \
    apk del .build-deps

EXPOSE 8125/udp 8092/udp 8094

COPY entrypoint.sh /entrypoint.sh
ENTRYPOINT ["/entrypoint.sh"]
CMD ["telegraf"]
