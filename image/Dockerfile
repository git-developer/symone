# syntax=docker/dockerfile:1
ARG ALPINE_VERSION=latest
FROM alpine:${ALPINE_VERSION}
ARG TARGETARCH
ARG SUPERCRONIC_VERSION=v0.2.33
RUN <<EOF
  set -eu
  apk update
  apk add --no-cache bash jo jq mosquitto-clients
  if [ "$(apk list raspberrypi)" ]; then
    apk add --no-cache raspberrypi
  fi
  if [ "${TARGETARCH-}" ]; then
    target_arch="${TARGETARCH}"
  else
    arch="$(arch)"
    case "${arch}" in
      x86_64)  target_arch=amd64 ;;
      *86)     target_arch=386   ;;
      aarch64) target_arch=arm64 ;;
      arm*)    target_arch=arm   ;;
      *) echo >&2 "Unsupported architecture: ${arch}" && exit 1 ;;
    esac
  fi
  location="/usr/local/bin/supercronic-linux-${target_arch}"
  wget -O "${location}" "https://github.com/aptible/supercronic/releases/download/${SUPERCRONIC_VERSION}/supercronic-linux-${target_arch}"
  chmod +x "${location}"
  ln -s "${location}" /usr/local/bin/supercronic
EOF
COPY ./ /opt/symone/
ENV PATH="${PATH}:/opt/symone"
CMD  [ "start-monitoring" ]
