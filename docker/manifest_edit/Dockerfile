ARG MP4SPLIT_IMAGE=ARG MP4SPLIT_IMAGE=unifiedstreaming/mp4split:1.7.32

FROM $MP4SPLIT_IMAGE

# ARGs declared before FROM are in a different scope, so need to be stated again
# https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG MP4SPLIT_IMAGE=ARG MP4SPLIT_IMAGE=unifiedstreaming/mp4split:1.7.32
ARG ALPINEVERSION=3.14
ARG REPO=http://artifact.internal.unified-streaming.com/last/apk/alpine/
ARG VERSION=1

# Install Manifest Edit
RUN apk \
    --update \
    --repository $REPO/v$ALPINEVERSION \
    add \
        manifest-edit~$VERSION \
        python3 \
        py3-pip \
&&  pip3 install \
        pyyaml==5.3.1 \
        schema==0.7.3 \
&&  rm -f /var/cache/apk/*

# Copy entrypoint script
COPY entrypoint.sh /usr/local/bin/entrypoint.sh

RUN chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/usr/local/bin/entrypoint.sh"]

WORKDIR /data

CMD []
