FROM golang:1.10.2
WORKDIR /go/src/app
COPY . .
RUN CGO_ENABLE=0 GOOS=linux go build -o vault-init -v .

FROM launcher.gcr.io/google/debian9:latest
COPY --from=0 /go/src/app/vault-init .

COPY . /app
WORKDIR /app
RUN apt-get update -y && apt-get install golang-go -y

ENTRYPOINT ["/vault-init"]
