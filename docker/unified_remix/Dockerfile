ARG MP4SPLIT_IMAGE=unifiedstreaming/mp4split:1.13.1

FROM $MP4SPLIT_IMAGE

# ARGs declared before FROM are in a different scope, so need to be stated again
# https://docs.docker.com/engine/reference/builder/#understand-how-arg-and-from-interact
ARG MP4SPLIT_IMAGE=unifiedstreaming/mp4split:1.13.1

# Copy entrypoint script
COPY entrypoint.sh /usr/local/bin/entrypoint.sh

RUN chmod +x /usr/local/bin/entrypoint.sh

ENTRYPOINT ["/sbin/tini", "--", "/usr/local/bin/entrypoint.sh"]

WORKDIR /data

CMD ["unified_remix"]
