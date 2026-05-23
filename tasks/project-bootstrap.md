# Project Bootstrap Task Layer

Use this task layer when creating, scaffolding, or standardizing a new project, application, service, package, or monorepo workspace.

## Default Stack

Unless the active task or user request says otherwise, default to:

- Runtime: Node.js
- API framework: Fastify
- Language: TypeScript
- Styling: Tailwind CSS
- Deployment: Fly.io
- Containerization: Docker
- Unit/integration testing: Jest
- Unit test coverage: Jest coverage with coverage thresholds
- Automation/end-to-end testing: Cypress
- Web performance testing: Lighthouse
- Load testing: k6
- Observability instrumentation: OpenTelemetry
- Free/open observability dashboards: Prometheus, Grafana, and Loki where useful
- CI/CD: GitHub Actions
- Persistence: DAO layer with support for MongoDB and either PostgreSQL or MySQL

## Required Inputs

- `IS_MONOREPO`: yes or no
- `SERVICES`: required when `IS_MONOREPO` is yes
- `SQL_DATABASE`: PostgreSQL or MySQL
- `APP_NAME` or service names
- Deployment target and Fly.io app naming strategy

If `IS_MONOREPO` is unknown, ask before scaffolding. If `IS_MONOREPO` is yes and `SERVICES` is missing, ask for the service list before creating files.

## Required Behavior

- Confirm the intended project shape before creating a large scaffold.
- Prefer simple, conventional structure over heavy framework abstraction.
- Use TypeScript throughout application, test, and build configuration.
- Include Fastify server setup with health check and structured application entrypoint.
- Include Tailwind setup only where a UI/frontend package exists or the requested project needs web styling.
- Include a DAO boundary that can support MongoDB and the selected SQL database without coupling business logic to driver-specific APIs.
- Provide Dockerfiles and Docker Compose where useful for local development.
- Include Jest configuration and sample tests.
- Include Jest coverage scripts, coverage reports, and sensible starter thresholds.
- Include a documented red-green-refactor workflow for application code.
- Include Cypress configuration when the project includes a UI or browser automation surface.
- Include Lighthouse-based web performance testing when the project includes a web UI or public web surface.
- Include k6 load testing scaffolding for API, service, or user-flow load tests where useful.
- Include OpenTelemetry instrumentation hooks for server request tracing, metrics, and logs where appropriate.
- Include free/open observability dashboard scaffolding or documentation using Prometheus, Grafana, and Loki where useful.
- Include GitHub Actions workflow for install, lint/typecheck, test, build, and Docker validation where feasible.
- Include Fly.io configuration and deployment instructions.
- Generate detailed README documentation for local development, testing, performance testing, observability, Docker, CI/CD, and deployment.

## Monorepo Behavior

When `IS_MONOREPO` is yes:

- Ask for the list of services/packages before implementation if not provided.
- Create a workspace structure appropriate for the repository tooling.
- Keep shared code in a clearly named shared package only when at least two services need it.
- Give each service its own README section, environment variables, Docker target, tests, and Fly.io deployment notes.
- Give each service its own unit test coverage command and coverage threshold notes when applicable.
- Give each service its own performance testing and observability notes when applicable.
- Document how to run one service, all services, and dependency services locally.
- Document whether services deploy as separate Fly.io apps or as one app.

## Single-Project Behavior

When `IS_MONOREPO` is no:

- Create a focused application structure.
- Keep configuration local to the app.
- Avoid workspace complexity unless explicitly requested.
- Document local run, test, Docker, and Fly.io deployment commands for the single app.
- Document local unit test coverage commands and report output paths for the single app.
- Document local performance testing and observability commands for the single app when applicable.

## Persistence Expectations

- Create a DAO interface or repository boundary for persistence operations.
- Include MongoDB DAO implementation scaffolding.
- Include SQL DAO implementation scaffolding for PostgreSQL or MySQL based on `SQL_DATABASE`.
- Keep database configuration environment-driven.
- Do not hard-code credentials.
- Include local Docker Compose services for MongoDB and the selected SQL database when useful.

## Performance Testing Expectations

- Include Lighthouse scripts or documentation for web performance checks when a web UI or public web route exists.
- Include k6 scripts for API or service load testing when the scaffold exposes HTTP endpoints or background workflows.
- Keep default performance tests lightweight and safe for local development.
- Do not point load tests at production by default.
- Document target URLs, environment variables, thresholds, and how to run performance tests locally and in CI.
- Include optional GitHub Actions jobs for performance checks when they are fast and deterministic enough for CI.

## Unit Coverage Expectations

- Configure Jest coverage for unit and integration tests.
- Include `test:coverage` or equivalent package scripts.
- Generate text and machine-readable coverage reports where useful.
- Add starter coverage thresholds that are realistic for a new scaffold and easy to raise over time.
- Exclude generated files, build output, test fixtures, and configuration files from coverage.
- Run coverage in GitHub Actions when feasible.
- Document where coverage reports are written and how to interpret failures.

## Observability Expectations

- Include OpenTelemetry-friendly application structure for traces, metrics, and logs where appropriate.
- Prefer free/open observability tooling for local dashboards, such as Prometheus for metrics, Grafana for dashboards, and Loki for logs.
- Include Docker Compose services or documented setup steps for local observability dashboards when useful.
- Expose health and readiness endpoints for services.
- Document key service metrics, log format, trace context behavior, and dashboard access URLs.
- Keep observability optional and environment-driven so local development remains simple.

## Documentation Expectations

The generated project README must include:

- Project overview
- Architecture and folder structure
- Monorepo service list, if applicable
- Prerequisites
- Environment variables
- Local development commands
- Docker and Docker Compose commands
- Jest test commands
- Jest coverage commands and report locations
- TDD/red-green-refactor workflow
- Cypress test commands, if applicable
- Lighthouse web performance commands, if applicable
- k6 load test commands, if applicable
- Observability setup and dashboard access, if applicable
- GitHub Actions CI/CD overview
- Fly.io setup and deployment steps
- Database setup for MongoDB and PostgreSQL/MySQL
- Troubleshooting notes

## Validation Expectations

- Verify package scripts are coherent.
- Run formatting, typecheck, lint, tests, or build checks when available.
- Validate Jest coverage configuration and scripts by static inspection or command execution when feasible.
- Validate performance test scripts and observability configuration by static inspection or dry run when feasible.
- If dependencies cannot be installed or commands cannot run, explain why and validate by static inspection.
- Confirm generated documentation matches the actual scaffold.

## Output Expectations

- Project shape summary
- Monorepo status and services, if applicable
- Stack choices used
- Files created
- Commands to run locally
- Test and CI/CD summary
- Unit coverage summary
- Performance testing summary
- Observability dashboard summary
- Fly.io deployment summary
- Any assumptions or follow-up setup required
