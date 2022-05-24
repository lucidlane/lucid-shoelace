FROM node:14.19-bullseye as build-stage

RUN apt-get update \
    # dependencies for building Python packages
    && apt-get install -y git jq \
    # cleaning up unused files
    && apt-get purge -y --auto-remove -o APT::AutoRemove::RecommendsImportant=false \
    && rm -rf /var/lib/apt/lists/*

ARG LL_ENV
ARG LL_VERSION

WORKDIR /app
COPY package.json package-lock.json /app/
RUN npm install

COPY . .
RUN npm run build

FROM alpine:3.15.4 as production-stage
COPY --from=build-stage /app/docs /dist
