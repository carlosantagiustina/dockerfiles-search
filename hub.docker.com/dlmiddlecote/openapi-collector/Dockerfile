FROM python:3.7-slim

WORKDIR /

RUN pip3 install poetry

COPY poetry.lock /
COPY pyproject.toml /

# fake package to make Poetry happy
RUN mkdir /openapi_collector && touch /openapi_collector/__init__.py && touch /README.md

RUN poetry config settings.virtualenvs.create false && \
    poetry install --no-interaction --no-dev --no-ansi

FROM python:3.7-slim

WORKDIR /

# copy pre-built packages to this image
COPY --from=0 /usr/local/lib/python3.7/site-packages /usr/local/lib/python3.7/site-packages

# copy actual code
COPY openapi_collector /openapi_collector

ARG VERSION=dev
RUN sed -i "s/__version__ = .*/__version__ = '${VERSION}'/" /openapi_collector/__init__.py

ENTRYPOINT ["python3", "-m", "openapi_collector"]
