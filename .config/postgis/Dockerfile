FROM postgis/postgis:13-3.1

LABEL maintainer="Nate Currit"

ENV PGROUTING_VERSION 3.1.2
# ENV PGROUTING_SHA256 b32e50269c79d65cb31bc611473c2ff0f9948b1a15dcaeef077ffcdfbd1c2730

RUN set -ex \
 && apt-get update \
      && apt-cache showpkg postgresql-plpython3-13 \
      && apt-get install -y --no-install-recommends \
           postgresql-plpython3-13 \
 && apt-get install -y \
        libboost-atomic1.67.0 \
        libboost-chrono1.67.0 \
        libboost-graph1.67.0 \
        libboost-date-time1.67.0 \
        libboost-program-options1.67.0 \
        libboost-system1.67.0 \
        libboost-thread1.67.0 \
        libcgal13 \
 && apt-get install -y \
        build-essential \
        cmake \
        wget \
        libboost-graph-dev \
        libcgal-dev \
        libpq-dev \
        postgresql-server-dev-13 \
 && wget -O pgrouting.tar.gz "https://github.com/pgRouting/pgrouting/archive/v3.1.2.tar.gz" \
#  && echo "$PGROUTING_SHA256 *pgrouting.tar.gz" | sha256sum -c - \
 && mkdir -p /usr/src/pgrouting \
 && tar \
        --extract \
        --file pgrouting.tar.gz \
        --directory /usr/src/pgrouting \
        --strip-components 1 \
 && rm pgrouting.tar.gz \
 && cd /usr/src/pgrouting \
 && mkdir build \
 && cd build \
 && cmake .. \
 && make \
 && make install \
 && cd / \
 && rm -rf /usr/src/pgrouting \
 && apt-mark manual postgresql-13 \
 && apt-get purge -y --autoremove \
        build-essential \
        cmake \
        wget \
        libcgal-dev \
        libpq-dev \
        libboost-graph-dev \
        postgresql-server-dev-13 \
 && rm -rf /var/lib/apt/lists/*
RUN rm /docker-entrypoint-initdb.d/10_postgis.sh
COPY init-user-db.sh /docker-entrypoint-initdb.d/10-init-user-db.sh
COPY initdb-postgis.sh /docker-entrypoint-initdb.d/11-initdb-postgis.sh