FROM python:3.12-slim-bookworm AS base

WORKDIR /usr/src/app

# 1. Copy the requirements file
COPY src/recommendation/requirements.txt ./

RUN pip install --upgrade pip
# 2. FIX: Explicitly install setuptools so opentelemetry-bootstrap can find pkg_resources
RUN pip install -r requirements.txt setuptools

# 3. Copy the service code and the shared protobufs
COPY src/recommendation/ ./
COPY pb/ ./pb/

# 4. Now this will have the resources it needs to run
RUN opentelemetry-bootstrap -a install 

# 5. Fixed the 'LegacyKeyValueFormat' warning
ENV RECOMMENDATION_PORT=1010

ENTRYPOINT ["python", "recommendation_server.py"]
