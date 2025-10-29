# GitHub Actions Setup Guide

## Quick Start

This guide will help you set up and configure all GitHub Actions workflows for the Heavens Above project.

## Table of Contents
1. [Prerequisites](#prerequisites)
2. [Initial Configuration](#initial-configuration)
3. [Workflow Overview](#workflow-overview)
4. [Configuration Steps](#configuration-steps)
5. [Testing Workflows](#testing-workflows)
6. [Monitoring](#monitoring)

---

## Prerequisites

Before setting up GitHub Actions, ensure you have:

- [x] Repository with admin access
- [x] GitHub account with Actions enabled
- [x] Node.js 12.10.0 or later installed locally (for testing)
- [x] Git configured on your machine

---

## Initial Configuration

### Step 1: Enable GitHub Actions

1. Go to your repository on GitHub
2. Click **Settings** → **Actions** → **General**
3. Under "Actions permissions", select:
   - ✅ **Allow all actions and reusable workflows**
4. Under "Workflow permissions", select:
   - ✅ **Read and write permissions**
   - ✅ **Allow GitHub Actions to create and approve pull requests**
5. Click **Save**

### Step 2: Enable GitHub Pages

1. Go to **Settings** → **Pages**
2. Under "Build and deployment":
   - Source: Select **GitHub Actions**
3. Click **Save**

GitHub Pages will now be available at: `https://<username>.github.io/<repository>/`

### Step 3: Protect Main Branch

1. Go to **Settings** → **Branches**
2. Click **Add rule**
3. Branch name pattern: `main`
4. Enable:
   - ✅ Require a pull request before merging
   - ✅ Require status checks to pass before merging
   - ✅ Require conversation resolution before merging
5. Select status checks:
   - ✅ Lint and Test
   - ✅ Code Quality Analysis
   - ✅ Build Validation
6. Click **Create**

---

## Workflow Overview

### 7 Automated Workflows

| # | Workflow | File | Auto-Runs | Manual |
|---|----------|------|-----------|--------|
| 1 | **Continuous Integration** | `ci.yml` | ✅ | ✅ |
| 2 | **Deployment Pipeline** | `deploy.yml` | ✅ | ✅ |
| 3 | **Scheduled Tasks** | `scheduled-tasks.yml` | ✅ | ✅ |
| 4 | **Dependency Updates** | `dependency-updates.yml` | ✅ | ✅ |
| 5 | **Code Review** | `code-review.yml` | ✅ | ✅ |
| 6 | **Documentation** | `documentation.yml` | ✅ | ✅ |
| 7 | **Custom Integration** | `custom-integration.yml` | ✅ | ✅ |

---

## Configuration Steps

### Basic Setup (No Additional Configuration Required)

The following workflows work out of the box:

✅ **1. Continuous Integration** - Automatically runs on push and PRs
✅ **2. Code Review** - Automatically runs on PRs
✅ **6. Documentation** - Automatically builds and deploys docs

### Optional Configuration

#### For Heroku Deployment (Workflow #2)

1. **Get Heroku API Key:**
   ```bash
   heroku auth:token
   ```

2. **Add Secrets to GitHub:**
   - Go to **Settings** → **Secrets and variables** → **Actions**
   - Click **New repository secret**
   - Add these secrets:
     - `HEROKU_API_KEY` - Your Heroku API token
     - `HEROKU_APP_NAME` - Your Heroku app name
     - `HEROKU_EMAIL` - Your Heroku account email

3. **Uncomment Heroku Deployment:**
   - Edit `.github/workflows/deploy.yml`
   - Find the commented Heroku deployment section
   - Uncomment the deployment steps

#### For Snyk Security Scanning (Workflow #5)

1. **Get Snyk Token:**
   - Sign up at [snyk.io](https://snyk.io)
   - Go to Account Settings → API Token

2. **Add Secret:**
   - Settings → Secrets and variables → Actions
   - Add: `SNYK_TOKEN` with your Snyk API token

3. **Uncomment Snyk Step:**
   - Edit `.github/workflows/code-review.yml`
   - Uncomment the Snyk security scan step

#### Configure Dependabot (Already Configured)

Dependabot is configured via `.github/dependabot.yml`:
- ✅ Weekly npm updates (Mondays 9:00 AM UTC)
- ✅ Weekly GitHub Actions updates
- ✅ Automatic PR creation for updates

**Customize if needed:**
Edit `.github/dependabot.yml` to change:
- Update frequency
- PR limits
- Reviewers/assignees
- Labels

---

## Testing Workflows

### Test Workflow #1: CI (Continuous Integration)

```bash
# Make a small change
echo "# Test" >> README.md

# Commit and push
git add README.md
git commit -m "test: trigger CI workflow"
git push origin main
```

**Expected Result:**
- CI workflow runs automatically
- Tests run on Node.js 12.x, 14.x, 16.x, 18.x
- Code quality checks complete
- Build validation passes

**View Results:**
1. Go to **Actions** tab
2. Click on "Continuous Integration" workflow
3. View job results and artifacts

### Test Workflow #2: Deployment

```bash
# Push to main (if not already done)
git push origin main
```

**Expected Result:**
- Build job creates deployment package
- GitHub Pages deployment succeeds
- Site accessible at GitHub Pages URL

**Verify:**
- Check Actions tab for deployment status
- Visit `https://<username>.github.io/<repository>/`

### Test Workflow #3: Scheduled Tasks

**Manual Test:**
1. Go to **Actions** tab
2. Select "Scheduled Tasks"
3. Click **Run workflow**
4. Select task: "data-refresh"
5. Click **Run workflow**

**Expected Result:**
- Data refresh job runs
- Satellite data is fetched
- Artifacts are uploaded

### Test Workflow #4: Dependency Updates

**Wait for Automated Run:**
- Runs automatically every Monday at 9:00 AM UTC

**Or Test Manually:**
1. Actions → "Dependency Updates" → Run workflow

**Expected Result:**
- Outdated packages report generated
- Security audit completed
- PRs created for updates (if any)

### Test Workflow #5: Code Review

```bash
# Create a test PR
git checkout -b test-pr
echo "console.log('test');" >> src/utils.js
git add src/utils.js
git commit -m "feat: add test logging"
git push origin test-pr
```

**Create PR on GitHub:**
1. Go to repository
2. Click "Compare & pull request"
3. Create PR

**Expected Result:**
- Code quality checks run
- Security scan completes
- Coding standards verified
- PR size analyzed
- Comment posted with review summary

### Test Workflow #6: Documentation

```bash
# Update README
echo "## New Section" >> README.md
git add README.md
git commit -m "docs: update README"
git push origin main
```

**Expected Result:**
- Documentation builds
- Deploys to GitHub Pages
- Accessible at Pages URL + `/docs`

### Test Workflow #7: Custom Integration

**Test Release Notes:**
```bash
# Create a version tag
git tag -a v1.0.0 -m "Release v1.0.0"
git push origin v1.0.0
```

**Expected Result:**
- Release notes automatically generated
- GitHub Release created
- Release notes artifact uploaded

**Test Performance Analysis:**
1. Actions → "Custom Workflow Integration" → Run workflow
2. Select task: "performance-analysis"

**Expected Result:**
- Performance metrics collected
- Analysis report generated
- Artifacts uploaded

---

## Monitoring

### View Workflow Status

**Actions Tab Dashboard:**
1. Go to **Actions** tab
2. View all workflow runs
3. Filter by:
   - Workflow name
   - Branch
   - Status (success, failure, in progress)
   - Date range

### Check Specific Workflow

1. Click workflow name in left sidebar
2. View recent runs
3. Click on a run to see details
4. Expand jobs to see step-by-step logs

### Download Artifacts

1. Go to workflow run
2. Scroll to bottom
3. Find "Artifacts" section
4. Click artifact name to download

### Email Notifications

**Configure Notifications:**
1. Settings → Notifications
2. Under "Actions"
3. Choose notification preferences:
   - Only notify on failures
   - Notify on all workflow runs
   - Custom notifications

### Status Badges

Add to your README.md:

```markdown
![CI](https://github.com/<username>/<repo>/workflows/Continuous%20Integration/badge.svg)
![Deploy](https://github.com/<username>/<repo>/workflows/Deployment%20Pipeline/badge.svg)
```

---

## Scheduled Workflow Times

All times in UTC:

| Workflow | Task | Schedule | Cron |
|----------|------|----------|------|
| Scheduled Tasks | Data Refresh | Daily 2:00 AM | `0 2 * * *` |
| Scheduled Tasks | Maintenance | Monday 3:00 AM | `0 3 * * 1` |
| Scheduled Tasks | Backup | 1st of month 4:00 AM | `0 4 1 * *` |
| Scheduled Tasks | Cleanup | 1st of month 4:00 AM | `0 4 1 * *` |
| Dependency Updates | Check Updates | Monday 9:00 AM | `0 9 * * 1` |
| Custom Integration | Performance | Sunday 1:00 AM | `0 1 * * 0` |

**Convert to Your Timezone:**
- Use [worldtimebuddy.com](https://www.worldtimebuddy.com/) to convert UTC
- Adjust cron expressions if needed

---

## Common Commands

### Run Workflows Manually

```bash
# Using GitHub CLI (gh)
gh workflow run ci.yml
gh workflow run deploy.yml
gh workflow run scheduled-tasks.yml --field task_type=data-refresh
```

### View Workflow Runs

```bash
# List recent runs
gh run list

# View specific run
gh run view <run-id>

# View logs
gh run view <run-id> --log
```

### Download Artifacts

```bash
# List artifacts from a run
gh run view <run-id> --log

# Download artifacts
gh run download <run-id>
```

---

## Troubleshooting

### Workflow Not Running

**Check:**
1. ✅ GitHub Actions enabled?
2. ✅ Workflow file syntax correct?
3. ✅ Trigger conditions met?
4. ✅ Branch protection not blocking?

**Solution:**
- Validate YAML syntax: Use online YAML validator
- Check Actions tab for errors
- Review workflow logs

### Permission Denied

**Check:**
1. ✅ Workflow permissions set to "Read and write"?
2. ✅ Required permissions in workflow file?

**Solution:**
- Update permissions in Settings → Actions → General
- Add required permissions to workflow file

### Deployment Fails

**Check:**
1. ✅ GitHub Pages enabled?
2. ✅ Pages source set to "GitHub Actions"?
3. ✅ `pages: write` permission granted?

**Solution:**
- Enable GitHub Pages in Settings
- Check deployment logs in Actions tab

### Scheduled Workflow Not Running

**Note:** GitHub may delay scheduled workflows by up to 15 minutes during high load.

**Check:**
1. ✅ Repository has recent activity?
2. ✅ Workflow file in default branch?
3. ✅ Cron syntax correct?

**Solution:**
- Make a commit to trigger activity
- Validate cron expression: [crontab.guru](https://crontab.guru/)

---

## Next Steps

After setup:

1. **Monitor First Runs**
   - Check Actions tab daily for the first week
   - Review artifacts and reports
   - Fix any issues that arise

2. **Customize Workflows**
   - Adjust schedules to your needs
   - Add/remove steps as required
   - Configure optional integrations

3. **Team Training**
   - Share documentation with team
   - Demonstrate workflow features
   - Establish review processes

4. **Regular Maintenance**
   - Review security reports weekly
   - Update dependencies monthly
   - Monitor artifact storage usage

5. **Continuous Improvement**
   - Collect feedback from team
   - Optimize workflow performance
   - Add new automations as needed

---

## Resources

### Documentation
- [WORKFLOWS_DOCUMENTATION.md](./WORKFLOWS_DOCUMENTATION.md) - Complete workflow documentation
- [.github/workflows/README.md](./.github/workflows/README.md) - Workflow quick reference
- [GitHub Actions Docs](https://docs.github.com/en/actions) - Official documentation

### Tools
- [GitHub CLI](https://cli.github.com/) - Command-line tool for GitHub
- [Act](https://github.com/nektos/act) - Run Actions locally
- [Crontab Guru](https://crontab.guru/) - Cron expression helper

### Support
- GitHub Actions Status: [githubstatus.com](https://www.githubstatus.com/)
- GitHub Community: [github.community](https://github.community/)
- Project Issues: Report workflow issues in GitHub Issues

---

## Checklist

Use this checklist to ensure complete setup:

### Required Setup
- [ ] GitHub Actions enabled
- [ ] GitHub Pages enabled
- [ ] Main branch protection configured
- [ ] All workflow files committed
- [ ] First CI run successful
- [ ] Documentation deployed

### Optional Setup
- [ ] Heroku secrets configured (if using)
- [ ] Snyk token added (if using)
- [ ] Dependabot customized
- [ ] Email notifications configured
- [ ] Status badges added to README

### Testing
- [ ] CI workflow tested
- [ ] Deployment workflow tested
- [ ] Manual workflow run tested
- [ ] Pull request workflow tested
- [ ] Artifact download tested

### Documentation
- [ ] Team trained on workflows
- [ ] Documentation reviewed
- [ ] Troubleshooting guide accessible
- [ ] Contacts for support established

---

**Setup Complete! 🎉**

Your GitHub Actions workflows are now configured and ready to automate your development lifecycle.

---

**Version:** 1.0.0
**Last Updated:** 2025-10-30
**Support:** See [WORKFLOWS_DOCUMENTATION.md](./WORKFLOWS_DOCUMENTATION.md) for detailed help
