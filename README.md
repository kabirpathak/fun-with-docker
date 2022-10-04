Development lifecycle : 

### Commit #1 (Using docker-network and images)

- Pulled the following 2 images from dockerhub:
  - mongo 
  - mongo-express

- Created a new docker-network for mongo and mongo-express to be able to communicate with each other.
  - docker network create mongo-network
  - docker network ls
    ```
       NETWORK ID     NAME            DRIVER    SCOPE
       0a3fba826317   mongo-network   bridge    local
    ```

- Started the containers from the images : 
  - docker run -p 27017:27017 -d \
    -e MONGO_INITDB_ROOT_USERNAME=admin \
    -e MONGO_INITDB_ROOT_PASSWORD=password  \
    --name mongodb \
    --net mongo-network \
    mongo

  - docker run -d \
    -p 8081:8081 \
    -e ME_CONFIG_MONGODB_ADMINUSERNAME=admin \
    -e ME_CONFIG_MONGODB_ADMINPASSWORD=password \
    --net mongo-network \
    --name mongo-express \
    -e ME_CONFIG_MONGODB_SERVER=mongodb \
    mongo-express

    ```
    ME_CONFIG_MONGODB_SERVER=container_name only works here because mongo and mongo-express are in the same docker-network.
    ```

- Now mongo-express can be accessed from port 8081. Created database user-database.

- To check whether the connection between mongodb and mongo-express was correctly established, update the user from your browser, and then visit localhost:8081 -> db: user-database -> collection: users; You should be able to see 1 entry in the users collection.

<img src="https://user-images.githubusercontent.com/46262107/193707763-67df64c5-099b-44bd-92ad-ff73d1d9f1de.png" width="40%" />

---

### Commit #2 (Using docker-compose)

- Created a docker-compose file. No need to create a docker network, since docker-compose takes care of that.
- Running docker-compose -f mongo.yaml down will delete all created containers and docker network.
- Used healthcheck and depends_on to ensure that mongo-express starts only when mongodb is already up. Otherwise, mongo-express was trying to establish connection before mongodb was even up, and exiting.

---

### Commit #3 (Dockerfile)

- Created a Dockerfile for the application, using the following command : 
  ```docker build -t <name>:<version> . ```
  ```. here represents the location of the Dockerfile (loc of the directory)```

- Will need to change the url for connecting to mongo-db once the application image is used instead of just running the app locally. 
  ```mongodb://admin:password@localhost:27017 -> mongodb://admin:password@mongo```

---
