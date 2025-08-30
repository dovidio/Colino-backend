# Colino Backend

AWS Lambda functions for Google YouTube OAuth authentication proxy.

## Overview

This project provides two AWS Lambda functions that act as a proxy for Google OAuth authentication to obtain YouTube API tokens. These tokens can then be used by the colino command-line application to fetch user subscriptions and other YouTube data.
Colino is privacy-focused, so we won't store any user data or tokens. Instead, the API token is sent back to the command-line app so it can make authenticated requests directly to the YouTube API.

## Architecture

- **Lambda 1 (`auth_initiate`)**: Generates Google OAuth authorization URL
- **Lambda 2 (`auth_callback`)**: Handles OAuth callback and exchanges authorization code for tokens
- **Shared utilities**: Common code for configuration and HTTP response handling

## 🚀 Quick Start

### Prerequisites

- Python 3.9+
- Poetry
- AWS CLI configured
- Google Cloud Console project with YouTube API enabled

### Installation

1. Install dependencies:
```bash
make install
```

2. Set up environment variables:
```bash
cp .env.example .env
# Edit .env with your values
```

3. Set up development environment:
```bash
make setup-dev
```

## 🔄 CI/CD Pipeline

This project includes a complete CI/CD pipeline using GitHub Actions for automated testing and deployment.

### Quick Setup
1. **Configure GitHub Secrets** - See [CI/CD Setup Guide](docs/CI-CD-SETUP.md)
2. **Push to `develop`** - Automatically deploys to staging
3. **Push to `main`** - Automatically deploys to production

### Manual Deployment
```bash
# Deploy to development
make deploy-dev

# Deploy to staging
make deploy-staging

# Deploy to production
make deploy-production
```

For detailed CI/CD setup instructions, see **[docs/CI-CD-SETUP.md](docs/CI-CD-SETUP.md)**


📖 **Detailed OAuth Flow**: See **[docs/OAUTH-FLOW.md](docs/OAUTH-FLOW.md)** for complete authentication setup and troubleshooting.

## Project Structure

```
colino-backend/
├── src/
│   ├── lambdas/
│   │   ├── auth_initiate.py      # OAuth initiation Lambda
│   │   └── auth_callback.py      # OAuth callback Lambda
│   └── shared/
│       ├── config.py             # Configuration settings
│       ├── response_utils.py     # API response utilities
├── tests/
├── pyproject.toml
└── README.md
```

## Environment Variables

- `GOOGLE_CLIENT_ID`: Google OAuth client ID
- `GOOGLE_CLIENT_SECRET`: Google OAuth client secret
- `REDIRECT_URI`: OAuth callback URL
- `AWS_REGION`: AWS region
- `ALLOWED_ORIGINS`: CORS allowed origins

## Development

### Running Tests

```bash
poetry run pytest
```

### Code Formatting

```bash
poetry run black src/
poetry run flake8 src/
```

### Type Checking

```bash
poetry run mypy src/
```

## Security Considerations

- Store Google client credentials securely (AWS Secrets Manager recommended)
- Use least privilege IAM policies
- Enable CloudTrail logging
- Validate and sanitize all inputs
- Implement rate limiting

## License

This project is licensed under the MIT License.