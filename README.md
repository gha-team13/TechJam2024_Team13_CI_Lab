<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Readme - TechJam2024_Team13_CI</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            line-height: 1.6;
            margin: 20px;
        }
        h1, h2, h3 {
            color: #2c3e50;
        }
        h1 {
            border-bottom: 2px solid #ecf0f1;
            padding-bottom: 5px;
        }
        code {
            background-color: #ecf0f1;
            padding: 2px 5px;
            border-radius: 3px;
        }
        pre {
            background-color: #ecf0f1;
            padding: 10px;
            border-radius: 5px;
            overflow-x: auto;
        }
        ul {
            margin: 10px 0;
            padding-left: 20px;
        }
    </style>
</head>
<body>

    <h1>TechJam2024_Team13_CI_Lab</h1>

    <h2>Getting Started</h2>
    <p>The app is a simple <strong>React</strong> application built with <strong>Next.js</strong>, to demonstrate CI/CD workflows using GitHub Actions. It includes:</p>
    <ul>
        <li>Automated unit and end-to-end testing</li>
        <li>Linting</li>
        <li>Type-checking</li>
        <li>Formatting enforcement</li>
    </ul>
    <p>This app showcases DevOps practices for automating build, test, and deployment processes to improve development efficiency and code quality.</p>

    <h2>Workflows</h2>

    <h3>Workflow 1 - <code>team13*.yaml</code></h3>
    <p>This workflow is designed for testing and building a <strong>Next.js</strong> application. Below is an overview of its functionality:</p>

    <h4>Triggers</h4>
    <ul>
        <li>Can be triggered by another workflow using <code>workflow_call</code>.</li>
    </ul>

    <h4>Jobs</h4>
    <ul>
        <li><strong>Checkout Code</strong> - Fetches the repository code using <code>actions/checkout@v4</code>.</li>
        <li><strong>Setup Node.js Environment</strong> - Configures Node.js version 18 and enables npm caching for efficiency.</li>
        <li><strong>Install Dependencies</strong> - Installs required project dependencies using <code>npm ci</code>.</li>
        <li><strong>Code Quality Checks</strong> - Automatically fixes and checks code formatting, linting, and type errors.</li>
        <li><strong>Environment Setup for Build</strong> - Creates a <code>.env</code> file using the secret <code>FLAGSMITH_KEY</code>.</li>
        <li><strong>Build Application</strong> - Builds the Next.js application using <code>npm run build</code>.</li>
        <li><strong>Verify Build Output</strong> - Lists the contents of the <code>.next/</code> folder to confirm a successful build.</li>
        <li><strong>Upload Build Artifacts</strong> - Uploads the <code>.next/</code> build folder for future use.</li>
        <li><strong>Run Unit Tests</strong> - Executes unit tests with <code>npm run test</code>.</li>
        <li><strong>Upload Coverage Reports</strong> - Analyzes test coverage.</li>
        <li><strong>Report to Codecov</strong> - Reports coverage data to Codecov.</li>
        <li><strong>Setup Playwright</strong> - Installs and caches Playwright browsers for end-to-end testing.</li>
        <li><strong>Run End-to-End Tests</strong> - Executes E2E tests using Playwright.</li>
        <li><strong>Upload Playwright Reports</strong> - Saves Playwright test results for review.</li>
        <li><strong>Notify on Slack</strong> - Sends Slack notifications for failures.</li>
    </ul>

    <h3>Workflow 2 - <code>ci.yaml</code></h3>
    <p>This simpler workflow integrates the testing workflow defined in <code>team13*.yaml</code> for the Continuous Integration (CI) process.</p>

    <h4>Triggers</h4>
    <ul>
        <li>Triggered automatically when a pull request is opened or updated on the <code>main</code> branch.</li>
    </ul>

    <h4>Concurrency Control</h4>
    <p>Ensures only one workflow run per pull request is active at a time. If a new workflow starts, the previous one is canceled.</p>

    <h4>Jobs</h4>
    <ul>
        <li><strong>Reuse Team13 Workflow</strong> - Calls <code>team13*.yaml</code> to run its testing and build process.</li>
    </ul>

    <h2>Caching in the Workflows</h2>

    <h3>Node.js Dependency Caching</h3>
    <ul>
        <li><strong>Action:</strong> <code>actions/setup-node@v4</code></li>
        <li><strong>Purpose:</strong> Caches npm dependencies to avoid repeated downloads across workflow runs.</li>
        <li><strong>Benefit:</strong> Reduces the time spent installing dependencies, speeding up the workflow.</li>
    </ul>

    <h3>Playwright Browser Caching</h3>
    <ul>
        <li><strong>Action:</strong> <code>actions/cache@v3</code></li>
        <li><strong>Cache Key:</strong> <code>playwright-browsers-${{ env.PLAYWRIGHT_VERSION }}</code></li>
        <li><strong>Purpose:</strong> Saves Playwright browser binaries in the cache to avoid redundant downloads.</li>
        <li><strong>Behavior:</strong> Skips downloading and installing browsers if cached.</li>
        <li><strong>Fallback:</strong> If the cache is not found, runs <code>npx playwright install --with-deps</code> to install the browsers.</li>
    </ul>

</body>
</html>
