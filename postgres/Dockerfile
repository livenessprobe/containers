FROM ghcr.io/cloudnative-pg/postgresql:15

USER 0
RUN apt-get update \
        && apt-get install gnupg postgresql-common apt-transport-https lsb-release wget -y --no-install-recommends \
        && echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main" | tee /etc/apt/sources.list.d/timescaledb.list \
        && wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | apt-key add - \
        && apt-get update && apt-get install timescaledb-2-postgresql-15 timescaledb-toolkit-postgresql-15 -y --no-install-recommends \
        && rm -rf /var/lib/apt/lists/*

# Switch user back, same as base image
USER 26