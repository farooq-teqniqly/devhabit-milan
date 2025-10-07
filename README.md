# DevHabit

A developer habit tracking system with a modern React frontend and a pragmatic RESTful API backend.

## Overview

DevHabit is designed for developers to track their daily habits and progress in coding, learning, and productivity. The system supports:

- **Habit Creation & Management**: Define habits with different frequencies (daily, weekly, monthly)
- **Progress Tracking**: Record entries to track your habit completion
- **Batch Entry Import**: Import multiple entries via CSV
- **GitHub Integration**: Automatically track contributions via GitHub events
- **Tags**: Organize habits with customizable tags
- **Analytics**: View stats and charts to analyze your progress

## Architecture

- **Frontend**: React TypeScript application with Vite
- **Backend**: ASP.NET Core 9.0 Web API
- **Database**: PostgreSQL with Entity Framework Core
- **Testing**: Unit tests, Integration tests, and Functional tests
- **CI/CD**: GitHub Actions for automated builds and deployments
- **Monitoring**: Seq and Aspire Dashboard for observability
- **Deployment**: Azure Web Apps and Azure Static Web Apps

## Prerequisites

- .NET 9.0 SDK
- Node.js 20+
- PostgreSQL 17+
- Docker (optional, for local development)

## Quick Start

### Backend (API)

1. Navigate to the API project:

   ```bash
   cd DevHabit/DevHabit.Api
   ```

2. Run the application:

   ```bash
   dotnet run
   ```

The API will be available at `https://localhost:8081/swagger` or `http://localhost:8080/swagger`.

### Frontend (UI)

1. Navigate to the client project:

   ```bash
   cd client/devhabit-ui
   ```

2. Install dependencies:

   ```bash
   npm install
   ```

3. Start the development server:

   ```bash
   npm run dev
   ```

The application will be available at `http://localhost:5173`.

### Docker Development

For a complete local development environment:

1. Ensure Docker is running
2. Navigate to the API project directory:

   ```bash
   cd DevHabit
   ```

3. Run with Docker Compose:

   ```bash
   docker-compose up
   ```

This will start:

- DevHabit API on <https://localhost:8081> or <http://localhost:8080>
- PostgreSQL database on localhost:5432
- Seq logging dashboard on <http://localhost:8080:5341>
- Aspire Dashboard on <http://localhost:18888>

## Development Setup

### Database Setup

For local development, you can use the Docker PostgreSQL instance or set up your own PostgreSQL database.

The application will automatically run migrations on startup in development mode.

### Environment Variables

#### API

Create a User Secrets file at `%APPDATA%\Microsoft\UserSecrets\{Project-GUID}\secrets.json`:

```json
{
  "ConnectionStrings": {
    "Database": "Server=localhost;Database=devhabit;User Id=postgres;Password=postgres;Port=5432;",
    "IdentityDatabase": "Server=localhost;Database=devhabit_identity;User Id=postgres;Password=postgres;Port=5432;"
  },
  "Jwt": {
    "SecretKey": "your-256-bit-secret-key",
    "Issuer": "devhabit-api",
    "Audience": "devhabit-client"
  },
  "Encryption": {
    "Key": "your-32-byte-encryption-key"
  },
  "GitHub": {
    "ClientId": "your-github-app-client-id",
    "ClientSecret": "your-github-app-secret"
  },
  "GitHubAutomation": {
    "Enabled": true,
    "BatchSize": 50
  }
}
```

#### Client

Set the environment variable for the API base URL:

```bash
# In client/devhabit-ui/.env
VITE_API_BASE_URL=https://localhost:8081
```

## API Documentation

Once running, visit the Swagger UI at `https://localhost:8081/swagger` or `http://localhost:8080/swagger` to view the API documentation.

Key endpoints include:

- `/api/auth/login` - User authentication
- `/api/habits` - Habit management
- `/api/entries` - Entry tracking
- `/api/tags` - Tag management
- `/api/github` - GitHub integration

## Testing

### Choosing the Right Test Type

**Unit Tests** (`DevHabit.UnitTests`):

- Test individual components in isolation (services, validators, utilities)
- Mock external dependencies (database, HTTP clients, external APIs)
- Fast execution, should run in milliseconds
- Example: Testing a validator, service method, or utility function

**Integration Tests** (`DevHabit.IntegrationTests`):

- Test interactions between components (database operations, service-to-service communication)
- Use real database connection and external services
- Medium execution time, test complete workflows within subsystems
- Example: Testing API endpoints with database persistence, but not full HTTP requests

**Functional Tests** (`DevHabit.FunctionalTests`):

- Test end-to-end scenarios through the full HTTP stack
- Use real database and all external dependencies
- Slowest execution, validate complete user journeys
- Example: Testing user registration, login, and habit creation through API endpoints

### Backend Tests

From the `DevHabit` directory:

```bash
# Run all tests
dotnet test DevHabit.sln

# Run specific test types
dotnet test DevHabit.UnitTests        # Fast, isolated component tests
dotnet test DevHabit.IntegrationTests # Medium, component integration tests
dotnet test DevHabit.FunctionalTests  # Slow, end-to-end API tests

# Run with coverage (requires dotnet-coverage tool)
dotnet-coverage collect "dotnet test DevHabit.sln" -f cobertura -o coverage.xml
```

### Frontend Tests

```bash
cd client/devhabit-ui
npm run lint  # Static analysis and basic checks
```

## Deployment

The application uses GitHub Actions for CI/CD:

- **API Deployment**: Built and deployed to Azure Web Apps on changes to `DevHabit/` directory
- **Client Deployment**: Built and deployed to Azure Static Web Apps on changes to `client/devhabit-ui/` directory

### Manual Deployments

You can trigger deployments manually via GitHub Actions from the repository.

## Project Structure

```text
devhabit-milan/
├── README.md                    # This file
├── DevHabit/                    # Backend solution
│   ├── DevHabit.Api/           # Main API project
│   │   ├── Controllers/        # API endpoints
│   │   ├── Entities/           # Database entities
│   │   ├── DTOs/               # Data transfer objects
│   │   ├── Services/           # Business logic
│   │   ├── Database/           # EF Core setup
│   │   └── Jobs/               # Background jobs
│   ├── DevHabit.UnitTests/     # Unit tests
│   ├── DevHabit.IntegrationTests/ # Integration tests
│   └── DevHabit.FunctionalTests/  # Functional tests
├── client/
│   └── devhabit-ui/            # React frontend
│       ├── src/
│       │   ├── features/       # Feature components
│       │   ├── components/     # Shared UI components
│       │   ├── pages/          # Page components
│       │   └── api/            # API client
│       └── vite.config.ts      # Vite configuration
├── .github/workflows/          # CI/CD pipelines
└── docker-compose.yml          # Docker development setup
```

## Key Features

### Habit Management

- Create habits with custom names, descriptions, frequencies
- Support for different habit types and target values
- Milestone tracking for habit goals

### Entry Tracking

- Log habit completion entries with dates and notes
- Bulk entry creation for multiple dates
- CSV import functionality for historical data

### GitHub Integration

- Connect GitHub account to automatically create habits from repositories
- Automated tracking of contributions, commits, and issues
- OAuth-based secure authentication

### Analytics & Reporting

- Daily, weekly, and monthly progress statistics
- Contribution grid visualization
- Streaks and completion rate tracking

## Contributing

1. Fork the repository
2. Create a feature branch: `git checkout -b feature/new-feature`
3. Make your changes and ensure tests pass
4. Commit your changes: `git commit -am 'Add new feature'`
5. Push to the branch: `git push origin feature/new-feature`
6. Submit a pull request

## Technologies Used

### Backend

- .NET 9.0 (ASP.NET Core)
- Entity Framework Core
- PostgreSQL
- JWT Authentication
- Hangfire (background jobs)
- FluentValidation
- AutoMapper
- OpenTelemetry (observability)
- Refit (HTTP client)

### Frontend

- React 18
- TypeScript
- Vite
- Tailwind CSS
- React Router
- React Icons
- ESLint & Prettier

### Infrastructure

- Docker & Docker Compose
- GitHub Actions
- Azure Web Apps
- Azure Static Web Apps
- Seq (logging)
- Azure Aspire Dashboard

## License

This project is licensed under the MIT License - see the LICENSE file for details.
