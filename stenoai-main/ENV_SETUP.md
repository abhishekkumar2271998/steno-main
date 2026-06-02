# Environment Configuration Guide

## Overview
This project uses environment variables to store sensitive configuration and API keys. All secrets are managed through `.env` files and should **never be committed to version control**.

## Setup Instructions

### 1. Initial Setup
When you first clone the repository, copy the `.env.example` file:

```bash
cp .env.example .env
```

### 2. Configure Your Secrets
Edit the `.env` file and replace placeholder values with your actual credentials:

```bash
# PostHog Analytics Configuration
POSTHOG_API_KEY=your_posthog_api_key_here
POSTHOG_HOST=https://us.i.posthog.com

# Google Calendar OAuth2 Configuration
GOOGLE_CLIENT_ID=your_google_client_id_here
GOOGLE_CLIENT_SECRET=your_google_client_secret_here
GOOGLE_SCOPES=https://www.googleapis.com/auth/calendar.readonly
GOOGLE_AUTH_URL=https://accounts.google.com/o/oauth2/v2/auth
GOOGLE_TOKEN_URL=https://oauth2.googleapis.com/token

# Outlook Calendar OAuth2 Configuration (PKCE public client)
OUTLOOK_CLIENT_ID=your_outlook_client_id_here
OUTLOOK_SCOPES=Calendars.Read offline_access
OUTLOOK_AUTH_URL=https://login.microsoftonline.com/common/oauth2/v2.0/authorize
OUTLOOK_TOKEN_URL=https://login.microsoftonline.com/common/oauth2/v2.0/token
```

## Environment Variables Reference

| Variable | Description | Required | Example |
|----------|-------------|----------|---------|
| `POSTHOG_API_KEY` | PostHog analytics API key | Yes | `phc_...` |
| `POSTHOG_HOST` | PostHog server endpoint | Yes | `https://us.i.posthog.com` |
| `GOOGLE_CLIENT_ID` | Google OAuth2 client ID | Yes | `281073275073-...` |
| `GOOGLE_CLIENT_SECRET` | Google OAuth2 client secret | Yes | `GOCSPX-...` |
| `GOOGLE_SCOPES` | Google API scopes | Yes | `https://www.googleapis.com/auth/calendar.readonly` |
| `GOOGLE_AUTH_URL` | Google OAuth2 authorization endpoint | Yes | `https://accounts.google.com/o/oauth2/v2/auth` |
| `GOOGLE_TOKEN_URL` | Google OAuth2 token endpoint | Yes | `https://oauth2.googleapis.com/token` |
| `OUTLOOK_CLIENT_ID` | Outlook OAuth2 client ID | Yes | `53a8ba1f-...` |
| `OUTLOOK_SCOPES` | Outlook API scopes | Yes | `Calendars.Read offline_access` |
| `OUTLOOK_AUTH_URL` | Outlook OAuth2 authorization endpoint | Yes | `https://login.microsoftonline.com/common/oauth2/v2.0/authorize` |
| `OUTLOOK_TOKEN_URL` | Outlook OAuth2 token endpoint | Yes | `https://login.microsoftonline.com/common/oauth2/v2.0/token` |

## Security Best Practices

### ✅ Do's
- ✅ Store all secrets in the `.env` file
- ✅ Use `.env.example` as a template for developers
- ✅ Keep `.env` in `.gitignore` (already configured)
- ✅ Rotate secrets periodically
- ✅ Use strong, unique credentials for each service
- ✅ Share `.env.example` but never share `.env` files

### ❌ Don'ts
- ❌ Never commit `.env` files to git
- ❌ Never hardcode secrets in source code
- ❌ Never share `.env` files in chat, email, or issue trackers
- ❌ Never log or print secret values
- ❌ Never reuse the same secret across different environments

## Environment Fallbacks

The application has fallback values configured in `app/main.js`. If an environment variable is not set, the application will use the fallback value. **Important**: Fallback values are for development purposes only and should not be relied upon in production.

### Example in Code:
```javascript
const GOOGLE_CLIENT_ID = process.env.GOOGLE_CLIENT_ID || 'default_client_id';
```

## Different Environments

### Development (`.env`)
- Use for local development
- Can include test credentials
- Should be in `.gitignore`

### Production
- Use GitHub Secrets or cloud provider's secret management
- Never store production secrets in `.env` files
- Use GitHub Actions or CI/CD to inject secrets at runtime

## Troubleshooting

### "Undefined variable" errors
1. Verify the `.env` file exists in the project root
2. Check that environment variable names match exactly (case-sensitive)
3. Restart your application after modifying `.env`

### `.env` file not loading
- Ensure `dotenv` package is installed: `npm install dotenv`
- Verify the path is correct in `require('dotenv').config()`
- Check file permissions

## Related Files
- `.env` - Actual secrets (not in version control)
- `.env.example` - Template for developers
- `app/main.js` - Loads environment variables at startup
- `.gitignore` - Prevents `.env` from being committed

## Additional Resources
- [dotenv Documentation](https://github.com/motdotla/dotenv)
- [Environment Variables Best Practices](https://12factor.net/config)
- [OWASP Secret Management](https://cheatsheetseries.owasp.org/cheatsheets/Secrets_Management_Cheat_Sheet.html)
