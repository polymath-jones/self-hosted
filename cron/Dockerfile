ARG BASE_IMAGE
FROM ubuntu:22.04 as installer

# Install dependencies in a full Ubuntu image
USER 0
RUN apt-get update && apt-get install -y --no-install-recommends cron && \
    rm -r /var/lib/apt/lists/*

# Copy the entrypoint
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Now use the actual base image
FROM ${BASE_IMAGE}
USER 0

# Copy cron and entrypoint from the installer stage
COPY --from=installer /usr/sbin/cron /usr/sbin/cron
COPY --from=installer /usr/sbin/crond /usr/sbin/crond 2>/dev/null || true
COPY --from=installer /etc/cron* /etc/
COPY --from=installer /var/spool/cron /var/spool/cron
COPY --from=installer /entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]