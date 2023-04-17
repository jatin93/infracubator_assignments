# Given that you have instructions to run the go-app ( in pre-requiste )

# 1. Try to create a docker image out of it with the base image of golang:alpine
    dockerile to create a docker image of go-app
    
    FROM golang:alpine

    RUN apk add make
    COPY . ./go-app
    WORKDIR go-app
    RUN make build

    EXPOSE 8080
    ENTRYPOINT go run main.go

# 2 & 3. Run a container with that image and do a curl a request and make sure you are able to see the output. Tag the docker image with v1.
    docker build . -t jateensharma/go-app:v1
    docker run -p 8080:8080. -d jateensharma/go-app:v1
    curl http://localhost:8080/

# 4. Run docker history, observe and understand the output.
    docker history jateensharma/go-app:v1

# 5. Push the docker image to your dockerhub.
    docker push jateensharma/go-app:v1