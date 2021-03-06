FROM python:3.6-slim

ENV RASA_NLU_DOCKER="YES" \
    RASA_NLU_HOME=/app \
    RASA_NLU_PYTHON_PACKAGES=/usr/local/lib/python3.6/dist-packages

# Run updates, install basics and cleanup
# - build-essential: Compile specific dependencies
# - git-core: Checkout git repos
RUN apt-get update -qq \
    && apt-get install -y --no-install-recommends build-essential git-core \
    && apt-get clean \
    && rm -rf /var/lib/apt/lists/* /tmp/* /var/tmp/*

WORKDIR ${RASA_NLU_HOME}

RUN git clone https://github.com/wrathagom/rasa_nlu.git . \
    && git checkout docker-updates

RUN pip install -r alt_requirements/requirements_tensorflow_sklearn.txt
RUN pip install -r alt_requirements/requirements_spacy_sklearn.txt

RUN pip install -e .

RUN pip install https://github.com/explosion/spacy-models/releases/download/en_core_web_md-2.0.0/en_core_web_md-2.0.0.tar.gz --no-cache-dir > /dev/null \
    && python -m spacy link en_core_web_md en

COPY docker/config.yml config.yml

VOLUME ["/app/projects", "/app/logs", "/app/data"]

EXPOSE 5000

ENTRYPOINT ["./entrypoint.sh"]
CMD ["start", "-c", "config.yml", "--path", "/app/projects"]