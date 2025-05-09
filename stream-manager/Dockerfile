FROM alpine:3
LABEL Author="Rogger Fabri"

ENV SNAPSERVER_IP=127.0.0.1
ENV SNAPSERVER_PORT=1780
ENV PIP_NO_CACHE_DIR=1

RUN apk update && apk upgrade \
 && apk add --no-cache \
    dumb-init \
    py3-pip \
    python3 \
    tzdata \
 && mkdir -p ~/.config/pip && echo -e "[global]\nbreak-system-packages = true" | tee ~/.config/pip/pip.conf \
 && pip3 install --upgrade pip \
 && pip3 install apscheduler snapcast \
 && rm -rf /var/cache/apk/* /root/.cache 

COPY stream_manager.py stream_manager.py

RUN addgroup -S appgroup && adduser -S appuser -G appgroup
USER appuser

ENTRYPOINT ["dumb-init", "--"]
CMD [ "python3","-u","./stream_manager.py" ]

HEALTHCHECK --interval=5s --timeout=3s \
  CMD netstat -a | grep $SNAPSERVER_IP.*$SNAPSERVER_PORT.*ESTABLISHED