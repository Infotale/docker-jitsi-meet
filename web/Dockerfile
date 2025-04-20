# Install build deps + runtime deps
RUN apt-dpkg-wrap apt-get update && \
    apt-dpkg-wrap apt-get install -y \
      dnsutils cron nginx-extras socat curl jq \
      git make nodejs npm gnupg ca-certificates && \
    curl -fsSL https://deb.nodesource.com/setup_20.x | bash - && \
    apt-dpkg-wrap apt-get install -y nodejs && \
    rm -f /etc/nginx/conf.d/default.conf && \
    apt-cleanup

# Clone and build jitsi-meet from source
WORKDIR /tmp/jitsi-meet
RUN git clone https://github.com/Infotale/jitsi-meet.git . && \
    npm install && \
    make

# Copy build into final location
RUN rm -rf /usr/share/jitsi-meet && \
    mkdir -p /usr/share/jitsi-meet && \
    cp -r /tmp/jitsi-meet/build/* /usr/share/jitsi-meet/

# Save default config for later fallback
RUN mkdir -p /defaults && \
    cp /usr/share/jitsi-meet/interface_config.js /defaults/interface_config.js
