
FROM golang AS builder
WORKDIR /go/src/
COPY . /go/src/
RUN CGO_ENABLED=0 GOOS=linux GOARCH=amd64 go build -v -ldflags "-X main.version=$DOCKER_TAG" -a -tags netgo -o release/linux/amd64/drone-docker ./cmd/drone-docker

FROM docker:18.09.0-dind
WORKDIR /bin/
COPY --from=builder /go/src/release/linux/amd64/drone-docker .
ENTRYPOINT ["/usr/local/bin/dockerd-entrypoint.sh", "/bin/drone-docker"]
