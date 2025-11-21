# Frontend Application (React)

This directory contains the React frontend for the Task Manager application.

## Status: 🚧 To Be Developed

The frontend structure will include:

```
frontend/
├── public/
│   ├── index.html       # HTML template
│   └── favicon.ico      # Site icon
├── src/
│   ├── components/      # Reusable UI components
│   │   ├── TaskList.jsx
│   │   ├── TaskItem.jsx
│   │   ├── TaskForm.jsx
│   │   └── Header.jsx
│   ├── pages/           # Page components
│   │   ├── HomePage.jsx
│   │   └── NotFound.jsx
│   ├── services/        # API service layer
│   │   └── api.js
│   ├── hooks/           # Custom React hooks
│   ├── utils/           # Utility functions
│   ├── styles/          # CSS files
│   ├── App.jsx          # Main app component
│   └── index.js         # Entry point
├── tests/
│   └── components/      # Component tests
├── Dockerfile           # Container definition
├── nginx.conf           # Nginx configuration
├── .env.example         # Environment variables template
├── .eslintrc.json       # Linting configuration
├── package.json         # Dependencies and scripts
└── README.md            # This file
```

## Tech Stack
- React 18
- React Router - Navigation
- Axios - HTTP client
- Testing Library - Component testing
- ESLint - Code linting

## Features

### Task Management
- ✅ View all tasks in a list
- ✅ Create new tasks
- ✅ Edit existing tasks
- ✅ Delete tasks
- ✅ Filter by status (To-Do, In Progress, Done)
- ✅ Responsive design

## Development Commands

```bash
# Install dependencies
npm install

# Start development server
npm start

# Run tests
npm test

# Run tests with coverage
npm run test:coverage

# Build for production
npm run build

# Lint code
npm run lint

# Fix linting issues
npm run lint:fix
```

## Environment Variables

Create `.env` file based on `.env.example`:

```env
REACT_APP_API_URL=http://localhost:5000
REACT_APP_ENV=development
```

## Component Structure

### TaskList
Displays all tasks with filtering options

### TaskItem
Individual task card with edit/delete actions

### TaskForm
Form for creating and editing tasks

### Header
Application header with navigation

## Styling
- Modern, clean design
- Responsive layout
- Mobile-friendly

## Next Steps

1. Create React app: `npx create-react-app frontend`
2. Install dependencies
3. Create component structure
4. Implement API integration
5. Write component tests
6. Create Dockerfile
7. Configure Nginx
