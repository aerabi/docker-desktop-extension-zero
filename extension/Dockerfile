FROM node:17.7-alpine3.14 AS client-builder
WORKDIR /app/client
# cache packages in layer
COPY client/package.json /app/client/package.json
COPY client/yarn.lock /app/client/yarn.lock
ARG TARGETARCH
RUN yarn config set cache-folder /usr/local/share/.cache/yarn-${TARGETARCH}
RUN --mount=type=cache,target=/usr/local/share/.cache/yarn-${TARGETARCH} yarn
# install
COPY client /app/client
RUN --mount=type=cache,target=/usr/local/share/.cache/yarn-${TARGETARCH} yarn build

FROM alpine:3.15

LABEL org.opencontainers.image.title="Zero" \
    org.opencontainers.image.description="A sample extension that runs a shell script inside a container's Desktop VM." \
    org.opencontainers.image.vendor="Docker Inc." \
    com.docker.desktop.extension.api.version=">= 0.1.0" \
    com.docker.extension.screenshots="[{\"alt\":\"\",\"url\":\"https://bit.ly/3KPDKU6\"}]" \
    com.docker.extension.detailed-description="A sample extension that runs a shell script inside a container's Desktop VM." \
    com.docker.extension.publisher-url="https://aerabi.com" \
    com.docker.extension.additional-urls="[]" \
    com.docker.extension.changelog="" \
    com.docker.desktop.extension.icon="/img/Moby.png"

COPY hello.sh .
COPY metadata.json .
COPY --from=client-builder /app/client/dist ui
COPY img img

CMD [ "sleep", "infinity" ]
