
# Assignment 1

# start an nginx container & port forward to local and Check
    ```
     docker run -p 8080:80 -d nginx (docker run will pull image nginx if not already available)
    ```

# check logs
    ```
     docker logs <container name OR container id>
    ```

# go inside the container
     ```
     docker exec -it <container name OR container id> bash
     ```

# stop the container
     ```
     docker stop <container name OR container id >
     ```
