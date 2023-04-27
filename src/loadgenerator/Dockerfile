# Copyright The OpenTelemetry Authors
# SPDX-License-Identifier: Apache-2.0


FROM python:3.11-slim-buster as base

FROM base as builder
COPY debian-buster-sources.list /etc/apt/sources.list
RUN apt-get -qq update \
    && apt-get install -y --no-install-recommends g++ \
    && rm -rf /var/lib/apt/lists/*

COPY ./src/loadgenerator/requirements.txt .
RUN pip install -i https://pypi.tuna.tsinghua.edu.cn/simple --prefix="/reqs" -r requirements.txt

FROM base
WORKDIR /usr/src/app/
COPY --from=builder /reqs /usr/local
COPY ./src/loadgenerator/locustfile.py .
COPY ./src/loadgenerator/people.json .
ENTRYPOINT locust
