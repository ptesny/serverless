#FROM python:3.12.3-alpine3.20
#FROM python:3.12-bullseye
#FROM ubuntu:latest
FROM python:3.12.3-slim-bookworm

# Serverless
LABEL source = git@github.com:kyma-project/serverless.git

# build-base and linux-headers are needed to install all requirements
#RUN apk add --no-cache --virtual .build-deps build-base linux-headers

ADD ./python312/requirements.txt /kubeless/requirements.txt
ADD ./lib/*.py kubeless.py /
RUN chmod 644 /kubeless/requirements.txt \
&& chmod 644 /kubeless.py \
&& pip install --no-cache-dir -r /kubeless/requirements.txt \
&& ls -al /*.py \
&& sleep 20

WORKDIR /

USER 1000
# Tracing propagators are configured based on OTEL_PROPAGATORS env variable https://opentelemetry.io/docs/instrumentation/python/manual/#using-environment-variables
ENV OTEL_PROPAGATORS=tracecontext,baggage,b3multi
ENV OTEL_PYTHON_REQUESTS_EXCLUDED_URLS="healthz,favicon.ico,metrics"

CMD ["python", "/kubeless.py"]
