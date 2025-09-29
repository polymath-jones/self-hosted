
ARG BASE_IMAGE
FROM ${BASE_IMAGE}
USER 0

RUN ls -la /etc/
RUN ls -la /etc/apt/ || echo "/etc/apt/ does not exist"
COPY entrypoint.sh /entrypoint.sh

RUN if [ -n "${HTTP_PROXY}" ]; then mkdir -p /etc/apt && echo "Acquire::http::proxy \"${HTTP_PROXY}\";" >> /etc/apt/apt.conf; fi
RUN if [ -n "${HTTPS_PROXY}" ]; then mkdir -p /etc/apt && echo "Acquire::https::proxy \"${HTTPS_PROXY}\";" >> /etc/apt/apt.conf; fi
RUN if [ -n "${http_proxy}" ]; then mkdir -p /etc/apt && echo "Acquire::http::proxy \"${http_proxy}\";" >> /etc/apt/apt.conf; fi
RUN if [ -n "${https_proxy}" ]; then mkdir -p /etc/apt && echo "Acquire::https::proxy \"${https_proxy}\";" >> /etc/apt/apt.conf; fi
RUN apt-get update && apt-get install -y --no-install-recommends cron && \
    rm -r /var/lib/apt/lists/*

ENTRYPOINT ["/entrypoint.sh"]
