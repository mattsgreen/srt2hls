FROM debian:buster-slim

ARG gitbranch
ARG version
ARG buildnumber

COPY liquidsoap.key /tmp/liquidsoap.key

RUN echo " \
deb http://deb.debian.org/debian buster main contrib non-free\n \
deb http://security.debian.org/debian-security buster/updates main contrib non-free\n \
deb http://deb.debian.org/debian buster-updates main contrib non-free\n \
" > /etc/apt/sources.list \
    && apt-get update \
    && apt-get install -y \
       gnupg2 \
       ca-certificates \
       dumb-init \
    && apt-key add /tmp/liquidsoap.key \
    && echo "deb http://deb.liquidsoap.info/debian stable main" > /etc/apt/sources.list.d/liquidsoap.list \
    && apt-get update \
    && apt-get install -y \
       liquidsoap-"$gitbranch"=1:"$version"-"$buildnumber"~stable \
    && rm -rf /var/lib/apt/lists/* \
    && groupadd -g 2000 radio \
    && useradd -m -u 2001 -g radio radio

USER radio

ENTRYPOINT ["/usr/bin/dumb-init", "--", "liquidsoap"]

CMD ["/radio/live.liq"]
