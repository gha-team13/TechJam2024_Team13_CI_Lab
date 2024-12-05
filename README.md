- 👋 Hi, I’m @adcosta
- 👀 I’m interested in 
- 🌱 I’m currently competing in TechJam 2024
- 💞️ I’m looking to collaborate on GRC
- 📫 How to reach me anthony.dcosta@td.com
- 😄 Pronouns: he/him/his
- ⚡ Fun fact: GRC improves engagement

## Getting Started adcosta

The app is a simple React application built with Next.js, to demonstrate CI/CD workflows using GitHub Actions. It includes automated unit and end-to-end testing, linting, type-checking, and formatting enforcement. The app will be used to showcase DevOps practices for automating build, test, and deployment processes to improve development efficiency and code quality.

# TechJam2024_Team13_CI_Lab
Workflow 1 - team13*.yaml
This workflow is designed for testing and building a Next.js application. Here's how it works:

Triggers
The workflow can be triggered by another workflow using workflow_call.
Jobs
Checkout Code - Fetches the repository code using actions/checkout@v4.
Setup Node.js Environment - Configures Node.js version 18 and enables npm caching for efficiency.
Install Dependencies - Installs required project dependencies using npm ci.
Code Quality Checks - Automatically fixes and checks code formatting (format:fix and format).
Checks for linting errors using ESLint - Validates TypeScript by checking for type errors.
Environment Setup for Build - Creates a .env file using the secret FLAGSMITH_KEY.
Build Application - Builds the Next.js application using npm run build.
Verify Build Output - Lists the contents of the .next/ folder to confirm successful build.
Upload Build Artifacts - Uploads the .next/ build folder for future use if the build succeeds.
Run Unit Tests - Executes unit tests with npm run test.
Upload Coverage Reports - Uploads test coverage reports to analyze code testing coverage.
Report to Codecov - Reports coverage data to Codecov for detailed insights.
Setup Playwright, browser automation - Installs and caches Playwright browsers for end-to-end testing.
Run End-to-End tests  - Executes E2E tests using Playwright and the FLAGSMITH_KEY environment variable.
Upload Playwright Reports - Saves Playwright test results for review.
Notify on Slack, failures only - Sends a Slack notification with details of the failure.

Workflow 2 - ci.yaml
This workflow is simpler and integrates the testing workflow defined in the first file as part of the Continuous Integration (CI) process.

Triggers
Triggered automatically when a pull request is opened or updated on the main branch.
Concurrency Control - Ensures only one workflow run per pull request is active at a time. If a new workflow starts for the same pull request, the previous one is canceled.
Jobs
Reuse Team13 workflow - Calls the team13*.yaml to run its testing and build process, inherits secrets from the main repository. 

Caching in the Workflows

Node.js Dependency Caching
Action -  actions/setup-node@v4
Purpose - Caches npm dependencies to avoid downloading the same packages repeatedly across workflow runs.
Benefit -  Reduces the time spent installing dependencies, speeding up the workflow.

Playwright Browser Caching
Action -  actions/cache@v3
Cache Key - playwright-browsers-${{ env.PLAYWRIGHT_VERSION }}
Purpose - Saves the Playwright browser binaries in the cache.
Behavior - On subsequent runs, checks if the browser binaries exist in the cache.
If cached, it skips downloading and installing them.
Fallback - If the cache is not found (cache-hit != 'true'), it runs npx playwright install --with-deps to install the browsers again.
