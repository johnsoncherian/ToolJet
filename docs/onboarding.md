# ToolJet Developer Onboarding Guide

## Welcome to ToolJet!

This guide will help you get started as a contributor to the ToolJet open-source low-code framework. ToolJet enables developers to build internal tools quickly with a visual drag-and-drop interface.

## Project Overview

ToolJet consists of three main components:

- **Server**: NestJS-based API server (TypeScript)
- **Client**: React-based frontend application (JavaScript/JSX)
- **Plugins**: Extensible plugin system for data sources and integrations

## Prerequisites

### Required Software

- **Node.js**: Version 22.15.1 (see `package.json` engines)
- **npm**: Version 10.9.2
- **Docker**: For local development and testing
- **Git**: For version control

### Recommended Tools

- **IDE**: VS Code with TypeScript and React extensions
- **Database Client**: pgAdmin, DBeaver, or similar for PostgreSQL
- **API Client**: Postman or Insomnia for API testing

## Quick Start

### 1. Clone the Repository

```bash
git clone https://github.com/ToolJet/ToolJet.git
cd ToolJet
```

### 2. Install Dependencies

```bash
# Install root dependencies
npm install

# Install server dependencies
npm --prefix server install

# Install frontend dependencies  
npm --prefix frontend install

# Install plugins dependencies
npm --prefix plugins install
```

### 3. Environment Setup

Create environment files for local development:

```bash
# Copy example environment files
cp server/.env.example server/.env
cp frontend/.env.example frontend/.env
```

Configure the following key variables in `server/.env`:

```env
# Database Configuration
PG_HOST=localhost
PG_PORT=5432
PG_USER=postgres
PG_PASS=postgres
PG_DB=tooljet

# Application Configuration
SECRET_KEY_BASE=your-secret-key-here
LOCKBOX_KEY=your-lockbox-key-here

# Email Configuration (optional for local dev)
SMTP_HOST=localhost
SMTP_PORT=587
SMTP_USERNAME=
SMTP_PASSWORD=
```

### 4. Database Setup

```bash
# Create and setup database
npm run db:setup

# Seed with sample data (optional)
npm run db:seed
```

### 5. Start Development Servers

```bash
# Terminal 1: Start server
npm --prefix server run start:dev

# Terminal 2: Start frontend
npm --prefix frontend run start

# Terminal 3: Start plugins (if needed)
npm --prefix plugins run dev
```

The application will be available at:
- **Frontend**: http://localhost:8082
- **Server API**: http://localhost:3000

## Development Workflow

### Branching Strategy

ToolJet uses git-flow branching model:

- **`develop`**: Main development branch
- **`main`**: Stable production branch
- **Feature branches**: `feature/description`
- **Hotfix branches**: `hotfix/description`

### Making Changes

1. **Create Feature Branch**
   ```bash
   git checkout develop
   git pull origin develop
   git checkout -b feature/your-feature-name
   ```

2. **Make Changes**
   - Follow existing code patterns
   - Write tests for new functionality
   - Update documentation as needed

3. **Test Your Changes**
   ```bash
   # Run server tests
   npm --prefix server run test
   
   # Run frontend tests
   npm --prefix frontend run test
   
   # Run linting
   npm run lint
   ```

4. **Submit Pull Request**
   - Create PR against `develop` branch
   - Include clear description of changes
   - Reference any related issues

### Code Standards

**TypeScript/JavaScript:**
- Use ESLint configuration provided
- Follow existing naming conventions
- Add JSDoc comments for public APIs
- Write unit tests for business logic

**React Components:**
- Use functional components with hooks
- Follow component naming conventions
- Implement proper prop types
- Use SCSS for styling

**Database:**
- Use TypeORM entities
- Create migrations for schema changes
- Follow naming conventions for tables/columns

## Project Structure

```
ToolJet/
├── server/                 # NestJS API server
│   ├── src/
│   │   ├── entities/       # Database models
│   │   ├── modules/        # Feature modules
│   │   ├── migrations/     # Database migrations
│   │   └── ...
│   └── package.json
├── frontend/               # React application
│   ├── src/
│   │   ├── components/     # Reusable components
│   │   ├── pages/          # Page components
│   │   ├── services/       # API services
│   │   └── ...
│   └── package.json
├── plugins/                # Plugin system
│   ├── packages/           # Individual plugins
│   └── package.json
├── docs/                   # Documentation
└── deploy/                 # Deployment configurations
```

## Key Concepts

### Applications
- Visual applications built with drag-and-drop components
- Support for multiple pages and responsive design
- Real-time collaborative editing

### Data Sources
- Connect to external databases, APIs, and services
- Support for 50+ data sources including PostgreSQL, MongoDB, REST APIs
- Secure credential storage with encryption

### Components
- 45+ built-in UI components (Tables, Charts, Forms, etc.)
- Customizable properties and styling
- Event handling and data binding

### Queries
- Visual query builders for each data source
- Support for custom JavaScript/Python code
- Data transformation and manipulation

## Testing

### Running Tests

```bash
# All tests
npm test

# Server tests only
npm --prefix server run test

# Frontend tests only
npm --prefix frontend run test

# E2E tests
npm --prefix cypress-tests run test
```

### Test Coverage

```bash
# Generate coverage report
npm --prefix server run test:cov
```

## Debugging

### Server Debugging

```bash
# Start with debugger
npm --prefix server run start:debug
```

### Frontend Debugging

- Use React Developer Tools browser extension
- Enable source maps in development
- Use browser dev tools for network debugging

### Database Debugging

- Use database client to inspect data
- Check migration status: `npm run db:migrate:status`
- Reset database: `npm run db:reset`

## Common Issues

### Database Connection Issues
- Ensure PostgreSQL is running
- Check connection parameters in `.env`
- Verify database exists and is accessible

### Port Conflicts
- Default ports: Server (3000), Frontend (8082)
- Change ports in respective `.env` files if needed

### Dependency Issues
- Clear node_modules and reinstall: `rm -rf node_modules && npm install`
- Check Node.js version compatibility

## Getting Help

### Resources

- **Documentation**: https://docs.tooljet.ai
- **GitHub Issues**: https://github.com/ToolJet/ToolJet/issues
- **Slack Community**: https://tooljet.ai/slack
- **Discord**: [ToolJet Discord](https://discord.gg/tooljet)

### Contributing Guidelines

- Read [CONTRIBUTING.md](../CONTRIBUTING.md)
- Follow the [Code of Conduct](../CODE_OF_CONDUCT.md)
- Check existing issues before creating new ones
- Use descriptive commit messages

## Next Steps

1. **Explore the Codebase**: Start with the server modules and frontend components
2. **Run Tests**: Ensure all tests pass locally
3. **Make Small Changes**: Start with documentation or small bug fixes
4. **Join the Community**: Participate in Slack discussions
5. **Read ADRs**: Review Architecture Decision Records in `docs/adr/`

## Additional Resources

- [ToolJet Documentation](https://docs.tooljet.ai)
- [API Reference](https://docs.tooljet.ai/docs/tooljet-api)
- [Component Library](https://docs.tooljet.ai/docs/widgets/)
- [Data Sources](https://docs.tooljet.ai/docs/data-sources/)

Welcome to the ToolJet community! 🚀
