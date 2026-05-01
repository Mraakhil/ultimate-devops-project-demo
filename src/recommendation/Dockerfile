FROM python:3.12-slim-bookworm AS base

WORKDIR /usr/src/app

# 1. Copy requirements
COPY src/recommendation/requirements.txt ./

# 2. Upgrade pip and install setuptools/wheel in one clean shot
RUN pip install --no-cache-dir --upgrade pip setuptools wheel
RUN pip install --no-cache-dir -r requirements.txt

# 3. Copy service code and shared protobufs
COPY src/recommendation/ ./
COPY pb/ ./pb/

# 4. FIX: Use 'python -m' to run the bootstrap module directly. 
# This is much more reliable on Python 3.12 than calling the binary script.
RUN python -m opentelemetry.instrumentation.bootstrap -a install

# 5. Clean up the ENV warning from your logs
ENV RECOMMENDATION_PORT=1010

ENTRYPOINT ["python", "recommendation_server.py"]
