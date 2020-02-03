FROM alpine:3.9
LABEL maintainer "Fluentd developers <fluentd@googlegroups.com>"
LABEL Description="Fluentd docker image" Vendor="Fluent Organization" Version="1.7.4"

# Do not split this into multiple RUN!
# Docker creates a layer for every RUN-Statement
# therefore an 'apk delete' has no effect
RUN apk update \
 && apk add --no-cache \
        ca-certificates linux-headers \
        ruby ruby-irb ruby-etc ruby-webrick \
        tini \
 && apk add --no-cache --virtual .build-deps \
        build-base \
        ruby-dev gnupg \
 && echo 'gem: --no-document' >> /etc/gemrc \
 && gem install oj -v 3.3.10 \
 && gem install json -v 2.2.0 \
 && gem install async-http -v 0.46.3 \
 && gem install fluentd -v 1.7.4 \
 && gem install bigdecimal -v 1.3.5 \
 && gem install fluent-plugin-s3 -v 1.2.0 \
 && gem install fluent-plugin-aws-elasticsearch-service -v 2.1.0 \
 && gem install fluent-plugin-kubernetes_metadata_filter -v 2.4.0 \
 && gem install fluent-plugin-multi-format-parser -v 1.0.0 \
 && apk del .build-deps \
 && rm -rf /tmp/* /var/tmp/* /usr/lib/ruby/gems/*/cache/*.gem \
 && mkdir -p /fluentd/log \
 && mkdir -p /fluentd/etc /fluentd/plugins


COPY fluent.conf /fluentd/etc/
COPY entrypoint.sh /bin/
COPY config.d /fluentd/etc/


ENV FLUENTD_CONF="fluent.conf"

ENV LD_PRELOAD=""
EXPOSE 24224 5140

ENTRYPOINT ["tini",  "--", "/bin/entrypoint.sh"]
CMD ["fluentd"]
