FROM centos:7

ENV CENTRIFUGO_VERSION 1.7.6

ENV CENTRIFUGO_SHA256 29f5b4d42fb9049e6d2a776624481c0d9968d910bf4388095df22d85589f6035

ENV CENTRIFUGO_DOWNLOAD https://github.com/centrifugal/centrifugo/releases/download/v$CENTRIFUGO_VERSION/centrifugo-$CENTRIFUGO_VERSION-linux-amd64.zip

RUN curl -sSL "$CENTRIFUGO_DOWNLOAD" -o /tmp/centrifugo.zip && \
    echo "${CENTRIFUGO_SHA256}  /tmp/centrifugo.zip" | sha256sum -c - && \
    yum install -y unzip && \
    unzip -jo /tmp/centrifugo.zip -d /tmp/ && \
    yum remove -y unzip && \
    mv /tmp/centrifugo /usr/bin/centrifugo && \
    rm -f /tmp/centrifugo.zip && \
    echo "centrifugo - nofile 65536" >> /etc/security/limits.d/centrifugo.nofiles.conf

RUN groupadd -r centrifugo && useradd -r -g centrifugo centrifugo

RUN mkdir /centrifugo && chown centrifugo:centrifugo /centrifugo && \
    mkdir /var/log/centrifugo && chown centrifugo:centrifugo /var/log/centrifugo

VOLUME ["/centrifugo", "/var/log/centrifugo"]

WORKDIR /centrifugo

USER centrifugo

COPY config.json /centrifugo/config.json

CMD ["centrifugo", "--config", "/centrifugo/config.json"]
