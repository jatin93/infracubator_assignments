# Multistage dockerfile for go application

# 1. Build stage
    FROM golang as build-go
    RUN git clone https://github.com/docker-ninja/go-app.git
    WORKDIR /go/go-app
    RUN make build-linux

# 2. Run stage
    FROM golang:alpine
    WORKDIR /go/executable
    COPY --from=build-go /go/go-app/out/ /go/executable
    EXPOSE 8080
    CMD ["./go-app"]

# 3. Commands to run
    docker build . -t jateensharma/go-app/gomultistage:v1
    docker run -d -p 8080:8080 jateensharma/go-app/gomultistage:v1
    curl http://localhost:8080/