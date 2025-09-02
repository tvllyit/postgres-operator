ARG BASE_IMAGE=registry.opensource.zalan.do/library/ubuntu-22.04:latest
FROM ${BASE_IMAGE}
LABEL maintainer="Team ACID @ Zalando <team-acid@zalando.de>"

SHELL ["/bin/bash", "-o", "pipefail", "-c"]

RUN apt-get update \
    && apt-get install --no-install-recommends -y \
        apt-utils \
        ca-certificates \
        lsb-release \
        pigz \
        python3-pip \
        python3-setuptools \
        python3-venv \
        curl \
        jq \
        gnupg \
        gcc \
        libffi-dev \
        build-essential \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/cache/apt/*

RUN curl --silent https://www.postgresql.org/media/keys/ACCC4CF8.asc | gpg --dearmor | tee /etc/apt/trusted.gpg.d/pgdg.gpg > /dev/null \
    && sh -c 'echo "deb http://apt.postgresql.org/pub/repos/apt/ $(lsb_release -cs)-pgdg main" > /etc/apt/sources.list.d/pgdg.list' \
    && apt-get update \
    && apt-get install --no-install-recommends -y \
        postgresql-client-17 \
        postgresql-client-16 \
        postgresql-client-15 \
        postgresql-client-14 \
    && curl -sL https://aka.ms/InstallAzureCLIDeb | bash \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* \
    && rm -rf /var/cache/apt/*

ENV VIRTUAL_ENV=/opt/venv
RUN python3 -m venv $VIRTUAL_ENV
ENV PATH="$VIRTUAL_ENV/bin:$PATH"

RUN pip install --no-cache-dir --upgrade pip \
    && pip install --no-cache-dir awscli gsutil

COPY dump.sh ./

ENV PG_DIR=/usr/lib/postgresql

ENTRYPOINT ["/dump.sh"]
