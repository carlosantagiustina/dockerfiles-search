FROM golang:1.13-alpine AS build

ENV CGO_ENABLED=0
ENV GOOS=linux
ENV GOARCH=amd64

WORKDIR /go/src/github.com/salekseev/aws-assume-role
COPY . .

RUN apk add -U make git
RUN make assume-role && chmod +x assume-role


FROM scratch AS run

COPY --from=build /go/src/github.com/salekseev/aws-assume-role/assume-role /assume-role

ENTRYPOINT ["/assume-role"]
