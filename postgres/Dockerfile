FROM postgres:15.4-bullseye

ARG PG_MAJOR=15
LABEL org.opencontainers.image.description="This Docker image contains PostgreSQL and Barman Cloud based on Postgres 15.4-bullseye."

COPY requirements.txt /

# Install additional extensions
RUN set -xe; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
		"postgresql-${PG_MAJOR}-pgaudit" \
		"postgresql-${PG_MAJOR}-pg-failover-slots" \
	; \
	rm -fr /tmp/* ; \
	rm -rf /var/lib/apt/lists/*;

# Install barman-cloud
RUN set -xe; \
	apt-get update; \
	apt-get install -y --no-install-recommends \
		python3-pip \
		python3-psycopg2 \
		python3-setuptools \
	; \
	pip3 install --upgrade pip; \
# TODO: Remove --no-deps once https://github.com/pypa/pip/issues/9644 is solved
	pip3 install --no-deps -r requirements.txt; \
	rm -rf /var/lib/apt/lists/*;

# Install TimescaleDB
RUN apt-get update \
        && apt-get install gnupg postgresql-common apt-transport-https lsb-release wget -y --no-install-recommends \
        && echo "deb https://packagecloud.io/timescale/timescaledb/debian/ $(lsb_release -c -s) main" | tee /etc/apt/sources.list.d/timescaledb.list \
        && wget --quiet -O - https://packagecloud.io/timescale/timescaledb/gpgkey | apt-key add - \
        && apt-get update && apt-get install timescaledb-2-postgresql-${PG_MAJOR} timescaledb-toolkit-postgresql-${PG_MAJOR} -y --no-install-recommends \
        && rm -rf /var/lib/apt/lists/*

RUN apt-get update \
	&& apt-get install -y --no-install-recommends postgresql-${PG_MAJOR}-pg-stat-kcache \
	&& rm -rf /var/lib/apt/lists/*

# Change the uid of postgres to 26
RUN usermod -u 26 postgres
USER 26