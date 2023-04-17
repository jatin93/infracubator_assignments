# docker compose file for https://github.com/docker-ninja/go-app.git

    docker/Assignment5/docker-compose.yaml


# Build docker image & run container using docker-compose.yaml
    docker-compose up -d

# Health check to determine application up or not
    curl http://localhost:5000/

# docker compose down
    docker-compose down
