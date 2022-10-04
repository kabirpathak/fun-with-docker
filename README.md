Development lifecycle : 

### Commit #1

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

- Now mongo-express can be accessed from port 8081. Created database user-accounts

- To check whether the connection between mongodb and mongo-express was correctly established, update the user from your browser, and then visit localhost:8081 -> db: user_account -> collection: users; You should be able to see 1 entry in the users collection.
