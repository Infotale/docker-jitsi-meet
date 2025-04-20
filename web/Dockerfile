ARG JITSI_REPO=jitsi
ARG BASE_TAG=latest
FROM ${JITSI_REPO}/base:${BASE_TAG}

LABEL org.opencontainers.image.title="Jitsi Meet"
LABEL org.opencontainers.image.description="WebRTC compatible JavaScript application that uses Jitsi Videobridge to provide high quality, scalable video conferences."
LABEL org.opencontainers.image.url="https://jitsi.org/jitsi-meet/"
LABEL org.opencontainers.image.source="https://github.com/jitsi/docker-jitsi-meet"
LABEL org.opencontainers.image.documentation="https://jitsi.github.io/handbook/"

ADD https://raw.githubusercontent.com/acmesh-official/acme.sh/3.0.7/acme.sh /opt
COPY rootfs/ /

# Install system and build dependencies
RUN apt-dpkg-wrap apt-get update && \
    apt-dpkg-wrap apt-get install -y \
      dnsutils cron nginx-extras socat curl jq git make nodejs npm && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-dpkg-wrap apt-get install -y nodejs && \
    rm -f /etc/nginx/conf.d/default.conf && \
    apt-cleanup

# Clone and build jitsi-meet
WORKDIR /tmp/jitsi-meet
RUN git clone https://github.com/Infotale/jitsi-meet.git . && \
    npm install && \
    make

EXPOSE 80 443

VOLUME ["/config", "/usr/share/jitsi-meet/transcripts"]
