FROM golang:alpine

RUN apk add make
COPY . ./go-app
WORKDIR go-app
RUN make build

EXPOSE 8080
ENTRYPOINT go run main.go