# Running Todo App on Docker Playground
## Prerequisites
- Access to Docker Playground
- A GitHub account or Docker Hub account to login

### Steps to Deploy
1. Create a New Instance
    1. Go to Docker Playground
    2. Login with your Docker Hub/GitHub account
    3. Click "Start" to create a new session
    4. Click "Add New Instance" to create a new Docker host
2. Clone the Project
```bash
git clone <your-repository-url>
cd ToDo
```

3. Update Configuration Files
Update the MongoDB connection in `server.js`:
```javascript
mongoose.connect("mongodb://mongo:27017/todo");
```

4. Create Docker Network
```bash
docker network create todo-network
```

5. Build and Run Services
```bash
docker-compose up --build -d
```

6. Verify Services
Check if all containers are running:
```bash
docker ps
```

You should see three containers:
- todo-frontend
- todo-backend
- todo-mongodb

7. Access the Application
- Click on port 3000 in Docker Playground UI to access the frontend
- The backend API will be available on port 3001
- MongoDB runs on port 27017

8. Build Verification
1. All three containers should be running
2. Check container status with `docker ps`
3. Verify network creation with `docker network ls`
4. Confirm volume creation with `docker volume ls`
5. Access the frontend and backend on ports 3000 and 3001

9. Troubleshooting
View container logs:
```bash
docker logs todo-frontend
docker logs todo-backend
docker logs todo-mongodb
```

10. Stop Services
To stop all services:
```bash
docker-compose down
```

To stop and remove volumes:
```bash
docker-compose down -v
```

Note
- The frontend will be available on port 3000
- The backend API will be accessible on port 3001
- MongoDB data is persisted using Docker volumes

---

## Accessing MongoDB in Docker Playground
Here are two methods to access MongoDB while your application is running:

### Method 1: Open New Terminal in Docker Playground
1. Click on "ADD NEW INSTANCE" in Docker Playground to open a new terminal
2. Connect to the MongoDB container using:
```bash
docker exec -it todo-mongodb mongosh
```
3. Once connected, use these MongoDB commands:
```bash
// Show databases
show dbs

// Use todo database
use todo

// Show collections
show collections

// Query todos
db.todos.find()

// Exit MongoDB shell
exit
```

### Method 2: Using MongoDB Express UI
Add MongoDB Express to your `docker-compose.yml` for a web interface:
```bash
services:
  # ...existing services...
  
  mongo-express:
    image: mongo-express
    container_name: mongo-express
    ports:
      - "8081:8081"
    environment:
      - ME_CONFIG_MONGODB_SERVER=mongo
      - ME_CONFIG_MONGODB_PORT=27017
    networks:
      - todo-network
    depends_on:
      - mongo
```
Then:

1. Run `docker-compose up -d mongo-express` in a new instance
2. Access MongoDB Express UI at port 8081 in Docker Playground
3. Click on port 8081 in the Docker Playground UI
4. Use the web interface to browse your database

---
### Setting up API URL in Docker Playground

1. Get your Docker Playground IP:
```bash
# Run this in Docker Playground terminal
echo $(hostname -i)
```

2. Export the IP as an environment variable:
```bash
export DOCKER_PLAYGROUND_IP=$(hostname -i)
```

3. Rebuild and restart the containers:
```bash
docker-compose down
docker-compose up --build
```

4. Verify the environment variable in frontend container:
```bash
docker exec todo-frontend printenv REACT_APP_API_URL
```

server: {
		allowedHosts: true
	},