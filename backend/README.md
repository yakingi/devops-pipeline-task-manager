# Backend Application (Node.js + Express)

This directory contains the backend REST API for the Task Manager application.

## Status: 🚧 To Be Developed

The backend structure will include:

```
backend/
├── src/
│   ├── controllers/     # Request handlers
│   ├── models/          # Database models
│   ├── routes/          # API routes
│   ├── middleware/      # Express middleware
│   ├── services/        # Business logic
│   ├── utils/           # Utility functions
│   ├── config/          # Configuration
│   └── server.js        # Application entry point
├── tests/
│   ├── unit/            # Unit tests
│   ├── integration/     # Integration tests
│   └── fixtures/        # Test data
├── Dockerfile           # Container definition
├── .env.example         # Environment variables template
├── .eslintrc.json       # Linting configuration
├── package.json         # Dependencies and scripts
└── README.md            # This file
```

## Tech Stack
- Node.js 18+
- Express.js - Web framework
- PostgreSQL - Database
- Jest - Testing framework
- ESLint - Code linting

## API Endpoints (Planned)

### Tasks
- `GET /api/tasks` - List all tasks
- `GET /api/tasks/:id` - Get single task
- `POST /api/tasks` - Create new task
- `PUT /api/tasks/:id` - Update task
- `DELETE /api/tasks/:id` - Delete task

### Health
- `GET /health` - Health check endpoint

## Development Commands

```bash
# Install dependencies
npm install

# Start development server
npm run dev

# Run tests
npm test

# Run tests with coverage
npm run test:coverage

# Lint code
npm run lint

# Fix linting issues
npm run lint:fix
```

## Environment Variables

Create `.env` file based on `.env.example`:

```env
DATABASE_URL=postgresql://user:password@localhost:5432/taskmanager
PORT=5000
NODE_ENV=development
```

## Next Steps

1. Initialize npm project: `npm init -y`
2. Install dependencies
3. Create Express server
4. Define database models
5. Implement API routes
6. Write tests
7. Create Dockerfile
