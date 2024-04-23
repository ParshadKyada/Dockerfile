# Dockerfile
Docker File used for Cloud IA 

# Frontend Dockerfile

1. **FROM node:latest:** Specifies the base image as the latest version of Node.js available on Docker Hub.
2. **WORKDIR /app:** Sets the working directory inside the container to /app, which will be the location for all subsequent commands.
3. **COPY package.json .:** Copies the package.json file from the host machine (where the Docker build is initiated) into the container's /app directory.
4. **RUN npm install:** Executes npm install within the container to install dependencies specified in package.json. This step ensures all required Node.js modules are installed in the container.
5. **COPY . .:** Copies all files and directories from the host machine's current directory (where the Dockerfile is located) into the container's /app directory. This includes the application code.
6. **EXPOSE 3000:** Documents that the container will listen on port 3000 at runtime. It does not publish the port.
7. **CMD ["npm", "start"]:** Specifies the default command to run when the container starts. In this case, it runs npm start, assuming that there is a corresponding start script defined in the package.json file for starting the Node.js application.

# Backend Dockerfile

1. **FROM node:latest:** Specifies the base image as the latest version of Node.js available on Docker Hub.
2. **WORKDIR /app:** Sets the working directory inside the container to /app, which will be the location for all subsequent commands.
3. **COPY package.json .:** Copies the package.json file from the host machine (where the Docker build is initiated) into the container's /app directory.
4. **RUN npm install:** Executes npm install within the container to install dependencies specified in package.json. This step ensures all required Node.js modules are installed in the container.
5. **COPY . .:** Copies all files and directories from the host machine's current directory (where the Dockerfile is located) into the container's /app directory. This includes the application code.
6. **EXPOSE 5000:** Documents that the container will listen on port 5000 at runtime. It does not publish the port.
7. **CMD ["npm", "start"]:** Specifies the default command to run when the container starts. In this case, it runs npm start, assuming that there is a corresponding start script defined in the package.json file for starting the Node.js application.

# Compose File

- **Services:**
        - **frontend:**
            - **build: ./frontend:** Specifies the path to the frontend application's directory containing a Dockerfile for building the frontend service.
            - **container_name: react-app-frontend:** Sets a custom container name (react-app) for the frontend service.
            - **ports: ["3000:3000"]:** Maps port 3000 on the host machine to port 3000 in the frontend container for accessing the frontend application.
            - **depends_on: - backend:** Defines a dependency on the backend service, ensuring the backend service starts before the frontend service.
            - **networks: - mern-network:** Connects the frontend service to the custom bridge network mern-network for inter-service communication.
        - **backend:** Similar configuration to frontend but for the backend service.
            - **environment: MONGODB_URI:**mongodb://database:27017/mydatabase: Sets the MONGODB_URI environment variable to specify the MongoDB connection URI, using the service name database as the hostname within the Docker network.
        - **database:**
            - **image: mongo:latest:** Uses the latest MongoDB image from Docker Hub.
            - **container_name: mongo-db:** Sets a custom container name (mongo-db) for the MongoDB service.
            - **ports: ["27017:27017"]:** Maps port 27017 on the host machine to port 27017 in the MongoDB container for accessing MongoDB.
            - **volumes: - mongodb-data:/data/db:** Mounts a named volume (mongodb-data) to persist MongoDB data outside the container.
        - **Networks:**
            - **mern-network:** Defines a custom bridge network (mern-network) for communication between services.
        - **Volumes:**
            - **mongodb-data:** Defines a named volume (mongodb-data) for persisting MongoDB data.


