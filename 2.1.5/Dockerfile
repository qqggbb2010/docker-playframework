FROM java:7

ENV PLAY_VERSION 2.1.5

MAINTAINER Jarle Fosen <jarlefosen@gmail.com>

RUN wget -q https://downloads.typesafe.com/play/2.1.5/play-${PLAY_VERSION}.zip \
    && unzip -q play-${PLAY_VERSION}.zip \
    && rm play-${PLAY_VERSION}.zip \
    && ln -s /play-${PLAY_VERSION}/play /usr/local/bin/
