ARG ALPINEVERSION=3.18

FROM alpine:$ALPINEVERSION

# ARGs declared before FROM are in a different scope, so need to be stated again
# https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG ALPINEVERSION
ARG REPO=https://beta.apk.unified-streaming.com/alpine/
ARG VERSION=1.13.4

# remove expired DST root if alpine 3.8
RUN if [ "$ALPINEVERSION" = "3.8" ]; then sed -i '2185,2226d' /etc/ssl/cert.pem; fi

# Get USP public key
RUN wget -q -O /etc/apk/keys/alpine@unified-streaming.com.rsa.pub \
        https://stable.apk.unified-streaming.com/alpine@unified-streaming.com.rsa.pub

# Install mp4split
RUN apk \
    --update \
    --repository $REPO/v$ALPINEVERSION \
    add \
        mp4split~$VERSION \
        mp4split-ffmpeg-plugins~$VERSION \
        tini \
&&  rm -f /var/cache/apk/*

# Copy Transcoder Config for ffmpeg-usp
COPY ffmpeg-transcoders.usp /etc/ffmpeg-transcoders.usp

# Copy entrypoint script
COPY entrypoint.sh /usr/local/bin/entrypoint.sh

RUN chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/sbin/tini", "--", "/usr/local/bin/entrypoint.sh"]

WORKDIR /data

CMD ["mp4split", "--show_license"]
