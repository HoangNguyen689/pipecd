# syntax=docker/dockerfile:1
FROM --platform=$BUILDPLATFORM golang:1.24.1 AS builder

ARG TARGETOS
ARG TARGETARCH

WORKDIR /app

COPY go.* ./
RUN go mod download

COPY . ./

RUN make build/go MOD=launcher BUILD_OS=${TARGETOS} BUILD_ARCH=${TARGETARCH}

# https://github.com/pipe-cd/pipecd/pkgs/container/piped-base/321463679?tag=v0.50.0-26-ga8527d2
FROM ghcr.io/pipe-cd/piped-base@sha256:9960b45a5aa822ae45ca2966056d8d2e98795b51681df25afd1fecf96360981c

COPY --from=builder /app/.artifacts/launcher /usr/local/bin/launcher

ENTRYPOINT ["launcher"]
