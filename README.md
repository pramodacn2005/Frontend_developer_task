# Frontend Developer Task - Scalable Web App

A full-stack web application with authentication and dashboard features, built with React.js and Node.js/Express.

## Features

### Frontend
- React.js with modern hooks and context API
- Responsive design using TailwindCSS
- Form validation (client-side)
- Protected routes (login required for dashboard)
- User authentication (login/register)
- Dashboard with user profile
- CRUD operations on tasks
- Search and filter functionality
- Beautiful, modern UI/UX

### Backend
- Node.js/Express RESTful API
- MongoDB database integration
- JWT-based authentication
- Password hashing with bcrypt
- Server-side validation
- Error handling middleware
- Protected routes with authentication middleware

## Prerequisites

Before you begin, ensure you have the following installed:
- Node.js (v14 or higher)
- npm or yarn
- MongoDB (local installation or MongoDB Atlas account)

## Installation & Setup

### 1. Clone the repository
```bash
git clone <repository-url>
cd Frontend_Develpoer_Task
```

### 2. Install dependencies

Install all dependencies (root, backend, and frontend):
```bash
npm run install-all
```

Or install manually:
```bash
# Install root dependencies
npm install

# Install backend dependencies
cd backend
npm install

# Install frontend dependencies
cd ../frontend
npm install
```

### 3. Configure Environment Variables

#### Backend Configuration
Create a `.env` file in the `backend` directory:
```bash
cd backend
# Copy env.example to .env
# On Windows PowerShell:
Copy-Item env.example .env
# On Mac/Linux:
cp env.example .env
```

Edit `backend/.env`:
```env
PORT=5000
MONGODB_URI=mongodb://localhost:27017/frontend_task
JWT_SECRET=your_super_secret_jwt_key_change_this_in_production
NODE_ENV=development
```

**Note:** For MongoDB Atlas, use:
```
MONGODB_URI=mongodb+srv://username:password@cluster.mongodb.net/frontend_task
```

#### Frontend Configuration (Optional)
Create a `.env` file in the `frontend` directory if you need to change the API URL:
```env
REACT_APP_API_URL=http://localhost:5000/api
```

### 4. Start MongoDB

Make sure MongoDB is running on your system:
```bash
# On Windows (if installed as service, it should start automatically)
# Or start manually:
mongod

# On macOS/Linux
sudo systemctl start mongod
# or
mongod
```

### 5. Run the Application

#### Option 1: Run both frontend and backend together
```bash
npm run dev
```

#### Option 2: Run separately

Terminal 1 - Backend:
```bash
cd backend
npm run dev
```

Terminal 2 - Frontend:
```bash
cd frontend
npm start
```

The application will be available at:
- Frontend: http://localhost:3000
- Backend API: http://localhost:5000

## API Documentation

### Base URL
```
http://localhost:5000/api
```

### Authentication Endpoints

#### Register User
```http
POST /api/auth/register
Content-Type: application/json

{
  "name": "John Doe",
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "message": "User registered successfully",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "profile": {}
  }
}
```

#### Login User
```http
POST /api/auth/login
Content-Type: application/json

{
  "email": "john@example.com",
  "password": "password123"
}
```

**Response:**
```json
{
  "message": "Login successful",
  "token": "jwt_token_here",
  "user": {
    "id": "user_id",
    "name": "John Doe",
    "email": "john@example.com",
    "profile": {}
  }
}
```

### Profile Endpoints

#### Get User Profile
```http
GET /api/profile
Authorization: Bearer {token}
```

#### Update User Profile
```http
PUT /api/profile
Authorization: Bearer {token}
Content-Type: application/json

{
  "name": "John Updated",
  "profile": {
    "bio": "Software Developer",
    "phone": "+1234567890",
    "location": "New York, USA"
  }
}
```

### Tasks Endpoints

#### Get All Tasks
```http
GET /api/tasks?status=pending&priority=high&search=task
Authorization: Bearer {token}
```

Query Parameters:
- `status`: pending | in-progress | completed
- `priority`: low | medium | high
- `search`: search term for title/description

#### Get Single Task
```http
GET /api/tasks/:id
Authorization: Bearer {token}
```

#### Create Task
```http
POST /api/tasks
Authorization: Bearer {token}
Content-Type: application/json

{
  "title": "Complete assignment",
  "description": "Finish the frontend task",
  "status": "pending",
  "priority": "high"
}
```

#### Update Task
```http
PUT /api/tasks/:id
Authorization: Bearer {token}
Content-Type: application/json

{
  "title": "Updated title",
  "status": "completed"
}
```

#### Delete Task
```http
DELETE /api/tasks/:id
Authorization: Bearer {token}
```

## Testing with Postman

A Postman collection is included in the `docs` folder. Import `Frontend_Task_API.postman_collection.json` into Postman to test all endpoints.

### Setting up Postman Environment

1. Create a new environment in Postman
2. Add variables:
   - `base_url`: `http://localhost:5000/api`
   - `token`: (will be set automatically after login)

3. Run the "Register" or "Login" request first to get a token
4. The token will be automatically saved to the environment variable

## Project Structure

```
Frontend_Develpoer_Task/
├── backend/
│   ├── models/
│   │   ├── User.js
│   │   └── Task.js
│   ├── routes/
│   │   ├── auth.js
│   │   ├── profile.js
│   │   └── tasks.js
│   ├── middleware/
│   │   └── auth.js
│   ├── server.js
│   ├── package.json
│   └── .env
├── frontend/
│   ├── public/
│   ├── src/
│   │   ├── components/
│   │   │   ├── PrivateRoute.js
│   │   │   ├── TaskModal.js
│   │   │   └── ProfileModal.js
│   │   ├── context/
│   │   │   └── AuthContext.js
│   │   ├── hooks/
│   │   │   └── useAuth.js
│   │   ├── pages/
│   │   │   ├── Login.js
│   │   │   ├── Register.js
│   │   │   └── Dashboard.js
│   │   ├── services/
│   │   │   └── api.js
│   │   ├── App.js
│   │   ├── index.js
│   │   └── index.css
│   ├── package.json
│   └── tailwind.config.js
├── docs/
│   └── Frontend_Task_API.postman_collection.json
└── README.md
```

## Security Features

- Password hashing with bcrypt (salt rounds: 10)
- JWT token-based authentication
- Protected API routes with middleware
- Input validation (client & server-side)
- CORS configuration
- Environment variables for sensitive data

## Scaling Considerations for Production

### Frontend Scaling

1. **Code Splitting & Lazy Loading**
   - Implement React.lazy() for route-based code splitting
   - Use dynamic imports for heavy components
   - Reduce initial bundle size

2. **State Management**
   - Consider Redux or Zustand for complex state management
   - Implement proper caching strategies
   - Use React Query for server state management

3. **Performance Optimization**
   - Implement virtual scrolling for large lists
   - Add service workers for offline support
   - Optimize images and assets
   - Use CDN for static assets

4. **Error Handling & Monitoring**
   - Integrate error tracking (Sentry, LogRocket)
   - Implement proper error boundaries
   - Add analytics for user behavior

5. **Testing**
   - Unit tests with Jest and React Testing Library
   - Integration tests for critical flows
   - E2E tests with Cypress or Playwright

### Backend Scaling

1. **Database Optimization**
   - Add indexes on frequently queried fields
   - Implement database connection pooling
   - Use MongoDB sharding for horizontal scaling
   - Add Redis for caching frequently accessed data

2. **API Optimization**
   - Implement pagination for list endpoints
   - Add rate limiting (express-rate-limit)
   - Use compression middleware (compression)
   - Implement API versioning

3. **Security Enhancements**
   - Add helmet.js for security headers
   - Implement refresh tokens
   - Add request validation middleware
   - Use HTTPS in production
   - Implement CSRF protection

4. **Architecture**
   - Separate into microservices if needed
   - Use message queues (RabbitMQ, Redis) for async tasks
   - Implement load balancing
   - Add API gateway for routing

5. **Monitoring & Logging**
   - Implement structured logging (Winston, Pino)
   - Add health check endpoints
   - Use APM tools (New Relic, Datadog)
   - Set up alerts for critical errors

6. **Deployment**
   - Use containerization (Docker)
   - Implement CI/CD pipelines
   - Use cloud services (AWS, Azure, GCP)
   - Set up auto-scaling based on load

### Infrastructure

1. **Frontend Deployment**
   - Deploy to Vercel, Netlify, or AWS S3 + CloudFront
   - Use CDN for global distribution
   - Implement proper caching headers

2. **Backend Deployment**
   - Use container orchestration (Kubernetes, Docker Swarm)
   - Deploy to cloud platforms (AWS ECS, Heroku, Railway)
   - Use managed MongoDB (MongoDB Atlas)
   - Implement auto-scaling

3. **CI/CD Pipeline**
   - Automated testing on pull requests
   - Automated deployment on merge
   - Environment-specific configurations
   - Rollback capabilities

## Troubleshooting

### MongoDB Connection Issues
- Ensure MongoDB is running: `mongod` or check service status
- Verify connection string in `.env` file
- Check firewall settings if using remote MongoDB

### Port Already in Use
- Change PORT in backend `.env` file
- Update `REACT_APP_API_URL` in frontend `.env` accordingly

### CORS Errors
- Ensure backend CORS is configured correctly
- Check that frontend URL is allowed in CORS settings

### Authentication Issues
- Verify JWT_SECRET is set in backend `.env`
- Check token expiration (default: 7 days)
- Ensure Authorization header format: `Bearer {token}`

## Notes

- This project uses MongoDB for data persistence
- JWT tokens expire after 7 days
- Passwords are hashed using bcrypt with 10 salt rounds
- All API endpoints require authentication except `/api/auth/*`

## Author

Frontend Developer Task Submission

## License

This project is created for assignment purposes.

