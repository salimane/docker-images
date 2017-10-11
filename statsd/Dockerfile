FROM node:8.6.0-alpine
MAINTAINER Salimane Adjao Moustapha <me@salimane.com>

ENV STATSD_VERSION 0.8.0

ADD https://github.com/etsy/statsd/archive/v${STATSD_VERSION}.tar.gz /tmp/statsd.tar.gz
RUN mkdir -p /src && \
	tar -xzvf /tmp/statsd.tar.gz -C /src && \
    rm /tmp/statsd.tar.gz && \
	mv /src/statsd-${STATSD_VERSION} /src/statsd

COPY config.js Dockerfile /etc/statsd/

EXPOSE  8125/udp
CMD ["node", "/src/statsd/stats.js", "/etc/statsd/config.js"]

ARG BUILD_DATE
ARG VCS_URL
ARG VCS_REF
ARG VCS_REF_MSG
ARG VERSION

LABEL version="$VERSION" \
      name="salimane/statsd" \
      maintainer="me@salimane.com" \
      description="A docker image for statsd (https://github.com/etsy/statsd)." \
      org.label-schema.build-date="$BUILD_DATE" \
      org.label-schema.vcs-url="$VCS_URL" \
      org.label-schema.vcs-ref="$VCS_REF" \
      org.label-schema.vcs-ref-msg="$VCS_REF_MSG" \
      org.label-schema.version="$VERSION" \
      org.label-schema.distribution-scope="public" \
      org.label-schema.changelog-url="https://github.com/salimane/docker-images/releases" \
      org.label-schema.url="https://github.com/salimane/docker-images" \
      org.label-schema.run="docker run -d -e ENV_NAME=ENV_VALUE -p 8125:8125/udp --name statsd salimane/statsd:v0.8.0" \
      org.label-schema.environment.required="GRAPHITE_HOST, GRAPHITE_PORT" \
      org.label-schema.environment.optional="FLUSH_INTERVAL, PERCENT_TRESHOLD, DEBUG, LOG_LEVEL" \
      org.label-schema.dockerfile="/etc/statsd/Dockerfile" \
      org.label-schema.schema-version="1.0"
