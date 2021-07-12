FROM golang:alpine as builder

RUN apk update && \
    apk add --no-cache git ca-certificates && \
    update-ca-certificates

WORKDIR /app

COPY go.mod .

ENV GO111MODULE=on
RUN go mod download
RUN go mod verify

COPY . .

RUN CGO_ENABLED=0 GOOS=linux go build -ldflags="-w -s" -a -installsuffix cgo -o /go/bin/{{ cookiecutter.cmd_name }} ./cmd/{{ cookiecutter.cmd_name }}

# Using Google Distroless for a secure container
FROM gcr.io/distroless/base

COPY --from=builder /go/bin/{{ cookiecutter.cmd_name }} /go/bin/{{ cookiecutter.cmd_name }}

ENTRYPOINT [ "/go/bin/{{ cookiecutter.cmd_name }}" ]