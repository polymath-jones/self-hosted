ARG BASE_IMAGE
FROM ubuntu:22.04 as installer

# Install dependencies in a full Ubuntu image
USER 0
RUN apt-get update && apt-get install -y --no-install-recommends cron && \
    rm -r /var/lib/apt/lists/*

# Copy the entrypoint
COPY entrypoint.sh /entrypoint.sh
RUN chmod +x /entrypoint.sh

# Create necessary directories
RUN mkdir -p /etc/cron.d /var/spool/cron

# Now use the actual base image
FROM ${BASE_IMAGE}
USER 0

# Copy cron binaries and configuration from the installer stage
COPY --from=installer /usr/sbin/cron /usr/sbin/cron
COPY --from=installer /etc/cron.d /etc/cron.d
COPY --from=installer /etc/crontab /etc/crontab
COPY --from=installer /var/spool/cron /var/spool/cron
COPY --from=installer /entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/entrypoint.sh"]