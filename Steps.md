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


8. Important Environment Variables
The docker-compose file includes these configurations:
```yaml
frontend:
  environment:
    - REACT_APP_API_URL=http://localhost:3001

backend:
  environment:
    - PORT=3001
    - MONGO_URI=mongodb://mongo:27017/todo

```

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