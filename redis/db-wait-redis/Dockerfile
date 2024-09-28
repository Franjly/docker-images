FROM ubuntu

USER root

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN \
  apt-get -qq update \
  && \
  apt-get -qq install -y \
    redis-server \
  && apt-get remove -y \
    jq \
  && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
  && apt-get autoremove -y \
  && apt-get clean \
  && \
  rm -rf \
    /tmp/* \
    /var/lib/apt/lists/* \
    /var/tmp/ \
  && printf "umask %d" "${UMASK}" >> /etc/bash.bashrc

USER apps

COPY ./entrypoint.sh /entrypoint.sh
CMD ["/entrypoint.sh"]
