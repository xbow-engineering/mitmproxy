FROM python:3.15-trixie@sha256:f34e3385a59fd8de7919d46db4a38ab14c0e46928a11b59036a7ee206ac0c129 AS wheelbuilder

COPY mitmproxy-*-py3-none-any.whl /wheels/
RUN pip install wheel && pip wheel --wheel-dir /wheels /wheels/*.whl

FROM python:3.15-slim-trixie@sha256:8c13a3f37a6a5b71498228e902c4b9f51c1f7900d49fabf71f9a650c6120e0d0

RUN useradd -mU mitmproxy
RUN apt-get update \
    && apt-get install -y --no-install-recommends gosu nano \
    && rm -rf /var/lib/apt/lists/*

RUN mkdir /home/mitmproxy/.mitmproxy \
    && chown mitmproxy:mitmproxy /home/mitmproxy/.mitmproxy

COPY --from=wheelbuilder /wheels /wheels
RUN pip install --no-index --find-links=/wheels mitmproxy
RUN rm -rf /wheels

VOLUME /home/mitmproxy/.mitmproxy

COPY docker-entrypoint.sh /usr/local/bin/
ENTRYPOINT ["docker-entrypoint.sh"]

EXPOSE 8080 8081

CMD ["mitmproxy"]
