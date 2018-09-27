FROM pygmy/postgres:10

ARG REPMGR_VERSION=4.0.6
ARG REPMGR_SHA1=176fb053acd415306cad79483e868775fb4e3529

RUN apk add --no-cache curl \
    && apk add --no-cache --virtual .build-deps \
        make \
        gcc \
        postgresql-dev \
        libxslt-dev \
        libxml2-dev \
        libedit-dev \
        libc-dev \
    && curl -sSL  https://github.com/2ndQuadrant/repmgr/archive/v${REPMGR_VERSION}.tar.gz -o ${REPMGR_VERSION}.tar.gz \
    && echo "${REPMGR_SHA1}  ${REPMGR_VERSION}.tar.gz" | sha1sum -c - \
    && tar zxf ${REPMGR_VERSION}.tar.gz \
    && cd repmgr-${REPMGR_VERSION} \
    && ./configure \
    && make USE_PGXS=1 install \
    && cd .. \
    && rm -Rf repmgr-${REPMGR_VERSION} ${REPMGR_VERSION}.tar.gz \
    && apk del .build-deps

VOLUME /var/lib/postgresql/10/data

ADD entrypoint.sh /entrypoint.sh

ENTRYPOINT ["/sbin/tini", "--", "/entrypoint.sh"]

CMD ["postgres"]
