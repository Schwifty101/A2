# GitHub Actions Workflows

This directory contains all GitHub Actions workflows for the Heavens Above project.

## Quick Reference

### Available Workflows

| Workflow | Status Badge | Description |
|----------|--------------|-------------|
| **CI** | ![CI](https://github.com/PKUPI/heavens-above/workflows/Continuous%20Integration/badge.svg) | Continuous integration with linting and testing |
| **Deploy** | ![Deploy](https://github.com/PKUPI/heavens-above/workflows/Deployment%20Pipeline/badge.svg) | Automated deployment to GitHub Pages and Heroku |
| **Scheduled** | ![Scheduled](https://github.com/PKUPI/heavens-above/workflows/Scheduled%20Tasks/badge.svg) | Automated data refresh, maintenance, and backups |
| **Dependencies** | ![Dependencies](https://github.com/PKUPI/heavens-above/workflows/Dependency%20Updates/badge.svg) | Automated dependency management |
| **Code Review** | ![Code Review](https://github.com/PKUPI/heavens-above/workflows/Code%20Review%20Automation/badge.svg) | Automated code quality and security checks |
| **Docs** | ![Docs](https://github.com/PKUPI/heavens-above/workflows/Documentation%20Deployment/badge.svg) | Documentation building and deployment |
| **Custom** | ![Custom](https://github.com/PKUPI/heavens-above/workflows/Custom%20Workflow%20Integration/badge.svg) | Release notes and performance analysis |

## Workflow Files

### 1. `ci.yml` - Continuous Integration
**Triggers:** Push to main, Pull requests
**Purpose:** Ensures code quality across multiple Node.js versions

**Jobs:**
- Lint and Test (Node 12.x, 14.x, 16.x, 18.x)
- Code Quality Analysis
- Build Validation

### 2. `deploy.yml` - Deployment Pipeline
**Triggers:** Push to main, Version tags
**Purpose:** Automates deployment to hosting platforms

**Jobs:**
- Build Application
- Deploy to GitHub Pages
- Deploy to Heroku (configurable)
- Post-Deployment Verification
- Deployment Notifications

### 3. `scheduled-tasks.yml` - Scheduled Tasks
**Triggers:** Daily, Weekly, Monthly schedules
**Purpose:** Automates maintenance and data refresh

**Jobs:**
- Data Refresh (Daily 2:00 AM UTC)
- System Maintenance (Monday 3:00 AM UTC)
- Data Backup (Monthly 1st 4:00 AM UTC)
- Cleanup (Monthly 1st 4:00 AM UTC)

### 4. `dependency-updates.yml` - Dependency Updates
**Triggers:** Weekly schedule, Pull requests affecting dependencies
**Purpose:** Manages and updates project dependencies

**Jobs:**
- Check Dependencies
- Security Audit
- Auto-update Minor Versions
- Validate Dependencies on PR
- License Compliance Check

### 5. `code-review.yml` - Code Review Automation
**Triggers:** Pull requests
**Purpose:** Automates code review checks

**Jobs:**
- Code Quality Check
- Security Scan (npm audit, CodeQL)
- Coding Standards Verification
- PR Size Analysis
- Test Coverage Analysis
- Review Summary

### 6. `documentation.yml` - Documentation Deployment
**Triggers:** Push/PR affecting docs or README
**Purpose:** Builds and deploys documentation

**Jobs:**
- Build Documentation (JSDoc, guides)
- Deploy Documentation (GitHub Pages)
- Validate Links
- Documentation Preview (PR only)

### 7. `custom-integration.yml` - Custom Workflow Integration
**Triggers:** Version tags, Weekly schedule, Manual
**Purpose:** Advanced automation tasks

**Jobs:**
- Generate Release Notes
- Performance Analysis (Sunday 1:00 AM UTC)
- Data Synchronization
- Workflow Summary

## Configuration Files

### `../.github/dependabot.yml`
Configures Dependabot for automated dependency updates:
- Weekly npm dependency checks
- Weekly GitHub Actions updates
- Groups minor/patch updates
- Applies appropriate labels

### `../../jsdoc.json`
Configures JSDoc for API documentation generation

## Getting Started

### Prerequisites
1. Enable GitHub Actions in repository settings
2. Enable GitHub Pages (Settings → Pages → Source: GitHub Actions)
3. Configure required secrets (optional, for Heroku/Snyk)

### Running Workflows Manually

1. Go to **Actions** tab in GitHub
2. Select the workflow you want to run
3. Click **Run workflow**
4. Select branch and options (if available)
5. Click **Run workflow**

### Viewing Workflow Results

1. Go to **Actions** tab
2. Click on a workflow run
3. View job details and logs
4. Download artifacts from the workflow run summary

### Monitoring Workflows

**Check workflow status:**
- Actions tab shows all workflow runs
- Status badges (see table above) show current status
- Email notifications for failures (configure in Settings → Notifications)

**Download artifacts:**
1. Go to workflow run
2. Scroll to "Artifacts" section
3. Click artifact name to download

## Common Tasks

### Add a New Workflow
1. Create new `.yml` file in `.github/workflows/`
2. Define workflow name and triggers
3. Add jobs and steps
4. Test with manual trigger
5. Document in this README

### Modify Existing Workflow
1. Edit the `.yml` file
2. Commit and push changes
3. Monitor the next workflow run
4. Update documentation if needed

### Troubleshoot Failed Workflow
1. Check workflow logs in Actions tab
2. Review error messages
3. Consult `WORKFLOWS_DOCUMENTATION.md` troubleshooting section
4. Enable debug logging if needed (add `ACTIONS_RUNNER_DEBUG` secret)

### Configure Secrets
1. Go to Settings → Secrets and variables → Actions
2. Click **New repository secret**
3. Add secret name and value
4. Secrets are available as `${{ secrets.SECRET_NAME }}`

## Artifact Retention

| Type | Retention Period |
|------|------------------|
| CI Reports | 30 days |
| Deployment Packages | 7 days |
| Security Reports | 90 days |
| Backups | 90 days |
| Release Notes | 365 days |
| Documentation | 90 days |

## Scheduled Workflow Times (UTC)

| Workflow | Schedule | Cron |
|----------|----------|------|
| Data Refresh | Daily 2:00 AM | `0 2 * * *` |
| Maintenance | Monday 3:00 AM | `0 3 * * 1` |
| Backup & Cleanup | 1st of month 4:00 AM | `0 4 1 * *` |
| Dependency Check | Monday 9:00 AM | `0 9 * * 1` |
| Performance Analysis | Sunday 1:00 AM | `0 1 * * 0` |

## Permissions

Workflows require the following permissions:

### Default Permissions
- `contents: read` - Read repository contents
- `pull-requests: write` - Comment on PRs

### Additional Permissions (specific workflows)
- `pages: write` - Deploy to GitHub Pages
- `id-token: write` - OIDC token for deployments
- `checks: write` - Update check runs
- `security-events: write` - Upload security findings
- `issues: write` - Create/comment on issues

## Best Practices

1. **Always test workflows** before pushing to main
2. **Use manual triggers** for testing
3. **Monitor workflow runs** regularly
4. **Review security reports** weekly
5. **Keep actions updated** (Dependabot helps)
6. **Set appropriate artifact retention** to save storage
7. **Document workflow changes** in commit messages

## Documentation

For comprehensive documentation, see:
- **[WORKFLOWS_DOCUMENTATION.md](../../WORKFLOWS_DOCUMENTATION.md)** - Complete workflow documentation
- **[GitHub Actions Docs](https://docs.github.com/en/actions)** - Official documentation

## Support

### Issues
Report workflow issues in the GitHub Issues tab with:
- Workflow name
- Run ID
- Error message
- Steps to reproduce

### Contributing
When modifying workflows:
1. Create a feature branch
2. Make changes
3. Test thoroughly
4. Update documentation
5. Submit pull request

## Compliance

All workflows follow:
- **KISS** - Keep It Simple, Stupid
- **YAGNI** - You Aren't Gonna Need It
- **DRY** - Don't Repeat Yourself
- **SOLID** - Object-oriented design principles

---

**Version:** 1.0.0
**Last Updated:** 2025-10-30
**Maintained by:** Project Team
