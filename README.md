

# 🚀 Deploying a MERN Stack Application with Docker Compose

This guide provides a comprehensive walkthrough for containerizing and deploying a MERN (MongoDB, Express.js, React, Node.js) stack application using Docker Compose. By the end of this tutorial, you'll have a fully functional MERN application running in isolated Docker containers. 🐳

## 🛠 Prerequisites

Before proceeding, ensure you have the following installed on your system:

- **Docker**: [Install Docker](https://docs.docker.com/get-docker/)
- **Docker Compose**: [Install Docker Compose](https://docs.docker.com/compose/install/)

### Create a network for the docker containers

`docker network create demo`

### Build the client 

```sh
cd mern/frontend
docker build -t mern-frontend .
```

### Run the client

`docker run --name=frontend --network=demo -d -p 5173:5173 mern-frontend`

### Verify the client is running

Open your browser and type `http://localhost:5173`

### Run the mongodb container

`docker run --network=demo --name mongodb -d -p 27017:27017 -v ~/opt/data:/data/db mongodb:latest`

### Build the server

```sh
cd mern/backend
docker build -t mern-backend .
```

### Run the server

`docker run --name=backend --network=demo -d -p 5050:5050 mern-backend`

## Using Docker Compose

`docker compose up -d`

Familiarity with the MERN stack and basic Docker concepts will be beneficial.

## 🗂 Project Structure

The application is organized as follows:

```
MERN-docker-compose/
├── mern/
│   ├── backend/
│   │   ├── Dockerfile
│   │   ├── package.json
│   │   └── ... (backend source files)
│   └── frontend/
│       ├── Dockerfile
│       ├── package.json
│       └── ... (frontend source files)
├── docker-compose.yml
└── README.md
```

- **`mern/backend/`**: Contains the Node.js and Express.js backend code along with its Dockerfile.
- **`mern/frontend/`**: Contains the React frontend code along with its Dockerfile.
- **`docker-compose.yml`**: Defines the services, networks, and volumes for the application.

## 📝 Understanding the `docker-compose.yml` File

The `docker-compose.yml` file orchestrates the services required for the MERN application. Let's break down its components:

```yaml
version: '3.8'  # Specifies the Docker Compose file format version

services:
  frontend:
    build:
      context: ./mern/frontend  # Path to the frontend directory
      dockerfile: Dockerfile    # Dockerfile to build the frontend image
    ports:
      - "5173:5173"             # Maps port 5173 on the host to port 5173 in the container
    depends_on:
      - backend                 # Ensures the backend service starts before the frontend
    networks:
      - mern-network            # Connects the frontend to the custom network

  backend:
    build:
      context: ./mern/backend   # Path to the backend directory
      dockerfile: Dockerfile    # Dockerfile to build the backend image
    ports:
      - "5050:5050"             # Maps port 5050 on the host to port 5050 in the container
    depends_on:
      - mongodb                 # Ensures the MongoDB service starts before the backend
    environment:
      MONGO_URI: mongodb://mongodb:27017/mydatabase  # Connection string for MongoDB
    networks:
      - mern-network            # Connects the backend to the custom network

  mongodb:
    image: mongo:latest         # Uses the latest MongoDB image
    ports:
      - "27017:27017"           # Maps port 27017 on the host to port 27017 in the container
    volumes:
      - mongo-data:/data/db     # Persists MongoDB data using a volume
    networks:
      - mern-network            # Connects MongoDB to the custom network

networks:
  mern-network:
    driver: bridge              # Creates a custom bridge network for inter-service communication

volumes:
  mongo-data:                   # Defines a volume for persisting MongoDB data
```

### Key Components Explained

- **`version: '3.8'`**: Specifies the Docker Compose file format version.
- **`services`**: Defines the three main services—`frontend`, `backend`, and `mongodb`.
  - **`build`**: Specifies the build context and Dockerfile location for building the images.
  - **`ports`**: Maps ports between the host and the containers.
  - **`depends_on`**: Establishes dependencies, ensuring services start in the correct order.
  - **`environment`**: Sets environment variables, such as the MongoDB connection string.
  - **`networks`**: Connects services to a custom network for seamless communication.
- **`networks`**: Defines a custom bridge network named `mern-network` to facilitate inter-service communication.
- **`volumes`**: Defines a volume `mongo-data` to persist MongoDB data across container restarts.

## 🚀 Deployment Steps

### 1. Clone the Repository

Begin by cloning the project repository:

```bash
git clone https://github.com/iam-veeramalla/MERN-docker-compose.git
cd MERN-docker-compose
```

### 2. Build and Start the Containers

Use Docker Compose to build and start all services defined in the `docker-compose.yml` file:

```bash
docker-compose up -d
```

- The `-d` flag runs the containers in detached mode, allowing them to operate in the background.

### 3. Verify the Deployment

After the containers are up and running:

- Access the frontend application at `http://localhost:5173`.
- Ensure that the backend is operational and connected to MongoDB.

### 4. Stop and Remove the Containers

To stop and remove all running containers, networks, and volumes defined in the `docker-compose.yml` file:

```bash
docker-compose down
```

## 🎯 Conclusion

By following this guide, you've successfully containerized and deployed a MERN stack application using Docker Compose. This approach ensures consistency across development and production environments, simplifies deployment, and enhances scalability. 🛠

For further reading and a step-by-step guide on deploying a MERN stack application using Docker Compose, refer to this article: [Step-by-Step Guide to Deploying a MERN Stack Application Using Docker Compose](https://medium.com/@ravipatel.it/step-by-step-guide-to-deploying-a-mern-stack-application-using-docker-compose-6e2dd7845b85)

## 📜 License

This project is licensed under the MIT License.

---

*Note: For a visual walkthrough and additional insights, consider watching the following tutorial:*

[Containerizing a MERN Stack Application and Deploying using Docker Compose | Step by Step Guide](https://www.youtube.com/watch?v=IUpsu2xemrA&utm_source=chatgpt.com) 



