# Operations Guide

## Runtime Profile

- Project: `vue-social-feed`
- Primary stack: Python
- File count at enhancement time: 94

## Local Development Checklist

- Install dependencies from `requirements.txt` or `pyproject.toml`.
- Start the API with the configured ASGI entry point.
- Run the test suite before pushing.

## Release Checklist

- Review `.env.example` and confirm required environment variables.
- Run tests and static validation before publishing.
- Review database migrations and seed data changes.
- Confirm health checks and CI workflows still reflect the runtime architecture.
- Update README and architecture notes if behavior changed.

## Troubleshooting

- Start with the generated CI workflow to see the intended build and test flow.
- Check environment variables first when authentication or connectivity fails.
- Validate database connectivity before debugging application-layer failures.
- Keep logs structured and avoid hiding infrastructure errors behind generic handlers.
