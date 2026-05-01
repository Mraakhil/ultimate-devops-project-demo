# Change version to 3.11
FROM python:3.11-slim-bookworm AS base

WORKDIR /usr/src/app

COPY src/recommendation/requirements.txt ./

RUN pip install --no-cache-dir --upgrade pip
RUN pip install --no-cache-dir -r requirements.txt

COPY src/recommendation/ ./
COPY pb/ ./pb/

# This command works perfectly on 3.11 without extra setuptools hacks
RUN opentelemetry-bootstrap -a install 

ENV RECOMMENDATION_PORT=1010

ENTRYPOINT ["python", "recommendation_server.py"]
