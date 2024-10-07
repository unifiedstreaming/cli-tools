ARG MP4SPLIT_IMAGE=unifiedstreaming/mp4split:1.14.4

FROM $MP4SPLIT_IMAGE

# ARGs declared before FROM are in a different scope, so need to be stated again
# https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG MP4SPLIT_IMAGE=unifiedstreaming/mp4split:1.14.4
ARG ALPINEVERSION=3.14
ARG REPO=https://stable.apk.unified-streaming.com/alpine/
ARG VERSION=1.14.4

# Install Manifest Edit
RUN apk \
    --update \
    --repository $REPO/v$ALPINEVERSION \
    add \
        manifest-edit~$VERSION \
&&  rm -f /var/cache/apk/*

# Copy entrypoint script
COPY entrypoint.sh /usr/local/bin/entrypoint.sh

RUN chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/sbin/tini", "--", "/usr/local/bin/entrypoint.sh"]

WORKDIR /data

CMD ["manifest_edit"]
