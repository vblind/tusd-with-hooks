FROM golang:1.21-alpine3.19 as builder

WORKDIR /go/src/github.com/tus/tusd

# Add gcc and libc-dev early so it is cached
RUN set -xe \
    && apk add --no-cache gcc libc-dev

# Install dependencies earlier so they are cached between builds
COPY go.mod go.sum ./
RUN set -xe \
    && go mod download

# Copy the source code, because directories are special, there are separate layers
COPY cmd/ ./cmd/
COPY internal/ ./internal/
COPY pkg/ ./pkg/


RUN go build -o tusd cmd/tusd/main.go

ENTRYPOINT ["./tusd", "-hooks-http", "http://vblind-static:8085/hooks-tusd", "-port", "8084", "-upload-dir=/uploads",\
    "-hooks-enabled-events", "pre-finish,pre-create,post-create,post-receive,post-finish,post-terminate"]
