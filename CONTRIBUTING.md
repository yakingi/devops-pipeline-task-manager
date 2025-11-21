# Contributing to DevOps Pipeline Task Manager

Thank you for your interest in contributing to our project! This document provides guidelines and best practices for contributing.

## Table of Contents
- [Code of Conduct](#code-of-conduct)
- [Getting Started](#getting-started)
- [Development Workflow](#development-workflow)
- [Commit Guidelines](#commit-guidelines)
- [Pull Request Process](#pull-request-process)
- [Coding Standards](#coding-standards)
- [Testing Requirements](#testing-requirements)

## Code of Conduct

### Our Pledge
- Be respectful and inclusive
- Welcome constructive feedback
- Focus on what's best for the team
- Show empathy towards other team members

### Unacceptable Behavior
- Harassment or discrimination
- Trolling or insulting comments
- Publishing others' private information
- Unprofessional conduct

## Getting Started

1. **Fork and Clone**
   ```bash
   git clone https://github.com/yakingi/devops-pipeline-task-manager.git
   cd devops-pipeline-task-manager
   ```

2. **Set Up Development Environment**
   - Follow the [Setup Guide](docs/SETUP_GUIDE.md)
   - Install all prerequisites
   - Verify local setup works

3. **Create a Feature Branch**
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/your-feature-name
   ```

## Development Workflow

### 1. Pick an Issue
- Check the [Project Board](../../projects)
- Assign yourself to an issue
- Move it to "In Progress"

### 2. Develop Locally
- Write code following our standards
- Test thoroughly
- Update documentation

### 3. Commit Changes
- Use conventional commit messages
- Make atomic commits
- Write clear commit descriptions

### 4. Push and Create PR
- Push to your feature branch
- Create Pull Request to `develop`
- Fill out the PR template completely

### 5. Code Review
- Address review feedback
- Keep PR updated with develop
- Resolve all conversations

### 6. Merge
- Squash and merge after approval
- Delete feature branch

## Commit Guidelines

We follow [Conventional Commits](https://www.conventionalcommits.org/) specification.

### Format
```
<type>(<scope>): <description>

[optional body]

[optional footer]
```

### Types
- `feat:` - New feature
- `fix:` - Bug fix
- `docs:` - Documentation only
- `style:` - Code style/formatting
- `refactor:` - Code refactoring
- `test:` - Adding/updating tests
- `chore:` - Maintenance tasks
- `ci:` - CI/CD changes
- `perf:` - Performance improvements

### Examples
```bash
feat(backend): add task creation endpoint

fix(frontend): resolve task deletion bug

docs(readme): update setup instructions

ci(workflow): add security scanning step
```

## Pull Request Process

### 1. PR Requirements
- ✅ All CI checks pass
- ✅ Code reviewed by at least 1 team member
- ✅ Code owner approval (if applicable)
- ✅ All conversations resolved
- ✅ Branch up to date with target
- ✅ Tests added/updated
- ✅ Documentation updated

### 2. PR Template
Fill out all sections:
- Description of changes
- Type of change
- Testing performed
- Checklist items

### 3. Review Process
- Reviewers have 48 hours to review
- Address all feedback
- Request re-review after changes
- Be open to suggestions

### 4. Merging
- Use "Squash and merge"
- Ensure commit message follows conventions
- Delete branch after merge

## Coding Standards

### JavaScript/Node.js
```javascript
// Use ES6+ features
const fetchTasks = async () => {
  try {
    const response = await fetch('/api/tasks');
    return await response.json();
  } catch (error) {
    console.error('Error fetching tasks:', error);
    throw error;
  }
};

// Use descriptive variable names
const isTaskComplete = task.status === 'done';

// Add JSDoc comments for functions
/**
 * Creates a new task
 * @param {Object} taskData - Task information
 * @param {string} taskData.title - Task title
 * @param {string} taskData.description - Task description
 * @returns {Promise<Object>} Created task object
 */
async function createTask(taskData) {
  // Implementation
}
```

### React
```javascript
// Use functional components with hooks
const TaskList = ({ tasks, onTaskClick }) => {
  const [filter, setFilter] = useState('all');
  
  const filteredTasks = useMemo(() => {
    return tasks.filter(task => 
      filter === 'all' || task.status === filter
    );
  }, [tasks, filter]);
  
  return (
    <div className="task-list">
      {filteredTasks.map(task => (
        <TaskItem 
          key={task.id} 
          task={task} 
          onClick={onTaskClick} 
        />
      ))}
    </div>
  );
};
```

### File Naming
- React components: `PascalCase.jsx`
- Utilities: `camelCase.js`
- Constants: `UPPER_SNAKE_CASE.js`
- Styles: `component-name.css`

### Code Organization
```
src/
├── components/      # Reusable components
├── pages/          # Page components
├── services/       # API services
├── utils/          # Utility functions
├── hooks/          # Custom hooks
├── constants/      # Constants
└── styles/         # Global styles
```

## Testing Requirements

### Backend Tests
```javascript
describe('Task API', () => {
  describe('GET /api/tasks', () => {
    it('should return all tasks', async () => {
      const response = await request(app).get('/api/tasks');
      expect(response.status).toBe(200);
      expect(Array.isArray(response.body)).toBe(true);
    });
  });
  
  describe('POST /api/tasks', () => {
    it('should create a new task', async () => {
      const taskData = {
        title: 'Test Task',
        description: 'Test Description'
      };
      
      const response = await request(app)
        .post('/api/tasks')
        .send(taskData);
        
      expect(response.status).toBe(201);
      expect(response.body).toHaveProperty('id');
    });
  });
});
```

### Frontend Tests
```javascript
describe('TaskList Component', () => {
  it('renders tasks correctly', () => {
    const tasks = [
      { id: 1, title: 'Task 1', status: 'todo' },
      { id: 2, title: 'Task 2', status: 'done' }
    ];
    
    render(<TaskList tasks={tasks} />);
    
    expect(screen.getByText('Task 1')).toBeInTheDocument();
    expect(screen.getByText('Task 2')).toBeInTheDocument();
  });
});
```

### Test Coverage Requirements
- Minimum 70% overall coverage
- All new features must include tests
- Bug fixes must include regression tests

### Running Tests
```bash
# Backend
cd backend
npm test
npm run test:coverage

# Frontend
cd frontend
npm test
npm run test:coverage
```

## DevOps Contributions

### Infrastructure Changes
- Test locally with `terraform plan`
- Document all changes
- Update variables and outputs
- Consider cost implications

### Ansible Changes
- Test playbooks in check mode first
- Use idempotent tasks
- Document role dependencies
- Update inventory as needed

### CI/CD Changes
- Test workflow changes in feature branch
- Document new environment variables
- Update secrets documentation
- Consider workflow runtime

## Documentation

### When to Update Docs
- New features added
- API changes
- Configuration changes
- Deployment process changes
- Troubleshooting solutions

### Documentation Standards
- Use clear, concise language
- Include code examples
- Add diagrams where helpful
- Keep setup guide current
- Update README when needed

## Questions?

- Create an issue with the question label
- Ask in team communication channel
- Check existing documentation
- Review similar PRs

Thank you for contributing! 🎉
