FROM ghcr.io/cloudnative-pg/postgresql:16.1-13

ARG PG_MAJOR=16

USER root

# Install TimescaleDB
RUN apt-get update \
        && apt-get install gnupg postgresql-common apt-transport-https lsb-release wget -y --no-install-recommends \
		&& yes | /usr/share/postgresql-common/pgdg/apt.postgresql.org.sh \
        && echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main" | tee /etc/apt/sources.list.d/timescaledb.list \
        && wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | apt-key add - \
        && apt-get update && apt-get install timescaledb-2-postgresql-${PG_MAJOR} -y --no-install-recommends \
        && rm -rf /var/lib/apt/lists/*

USER 26
