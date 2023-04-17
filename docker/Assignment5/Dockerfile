FROM golang:alpine

RUN apk add git
RUN apk add make
RUN apk add curl
RUN git clone https://github.com/docker-ninja/go-app.git
WORKDIR go-app
RUN make build

EXPOSE 8080
ENTRYPOINT go run main.go