# alpine >3.12 fails on raspbian buster due to https://gitlab.alpinelinux.org/alpine/aports/-/issues/12091 
FROM alpine:3.12
ARG TARGETARCH
ARG SUPERCRONIC_VERSION=v0.1.12

RUN  set -eu \
  && apk update \
  && apk add --no-cache bash jo jq mosquitto-clients \
  && { [ -z "$(apk list raspberrypi)" ] || apk add --no-cache raspberrypi ; } \
  && if [ -n "${TARGETARCH-}" ]; then target_arch="${TARGETARCH}"; else \
      arch="$(arch)" && case "${arch}" in \
       x86_64)  target_arch='amd64' ;; \
       *86)     target_arch='386'   ;; \
       aarch64) target_arch='arm64' ;; \
       arm*)    target_arch='arm'   ;; \
       *) >&2 echo "Unsupported architecture: ${arch}" && exit 1 ;; \
      esac \
     fi \
  && location="/usr/local/bin/supercronic-linux-${target_arch}" \
  && wget -O "${location}" "https://github.com/aptible/supercronic/releases/download/${SUPERCRONIC_VERSION}/supercronic-linux-${target_arch}" \
  && chmod +x "${location}" \
  && ln -s "${location}" /usr/local/bin/supercronic
COPY ./ /opt/symone/
ENV PATH="${PATH}:/opt/symone"
CMD  [ "start-monitoring" ]
