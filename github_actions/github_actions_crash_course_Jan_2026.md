---
marp: true
theme: gaia
class:
  - lead
paginate: true
backgroundColor: #89f0a5
---

# GitHub Actions Crash Course Jan 2026

---

## About Me

* Piti Champeethong (Fyi/ฟี่)
* Senior Consulting Engineer
* LinkedIn: [Me](https://www.linkedin.com/in/pitichampeethong/)

---

### Agenda

* **Part 1: Introduction to CI/CD and GitHub Actions (15 mins)**
* **Part 2: Core Concepts of GitHub Actions (20 mins)**
* **Part 3: Lab 1: Your First Workflow (15 mins)**
* **Part 4: Workflows, Jobs, and Steps in Detail (25 mins)**
* **Part 5: Lab 2: Building a Real CI Pipeline (20 mins)**
* **Break (15 mins)**

---

### Agenda (Continue)

* **Part 6: Actions and Reusable Workflows (20 mins)**
* **Part 7: Lab 3: Using Marketplace Actions (15 mins)**
* **Part 8: Secrets and Secure Workflows (10 mins)**
* **Part 9: Lab 4: Working with Secrets (10 mins)**
* **Part 10: Advanced Topics and Best Practices (10 mins)**
* **Part 11: Conclusion and Q&A (5 mins)**

---

### Part 1: Introduction to CI/CD and GitHub Actions

**What we will cover:**

* What is CI/CD?
* Why is it important?
* Introduction to GitHub Actions.

---

### What is Continuous Integration (CI)?

* The practice of merging all developers' working copies to a shared mainline several times a day.
* Each integration is verified by an automated build (including tests) to detect integration errors as quickly as possible.

---

### What is Continuous Delivery (CD)?

* A software engineering approach in which teams produce software in short cycles, ensuring that the software can be reliably released at any time.
* The goal is to make deployments predictable, routine affairs that can be performed on demand.

---

### The CI/CD Pipeline

A series of steps that must be performed in order to deliver a new version of software.

1. **Source:** A code change triggers the pipeline.
2. **Build:** The code is compiled.
3. **Test:** Automated tests are run.
4. **Deploy:** The application is deployed to an environment.

---

### Benefits of CI/CD

* Smaller Code Changes
* Fault Isolation
* Faster Mean Time To Resolution (MTTR)
* More Test Reliability
* Faster Release Rate
* Increased Team Transparency and Accountability

---

### What is GitHub Actions?

* An automation platform built into GitHub.
* Allows you to automate your software development workflows right in your repository.
* Primarily used for CI/CD, but can do much more!

---

### Why GitHub Actions?

* **Integrated:** Lives in your repository with your code.
* **Community-driven:** Thousands of reusable actions in the Marketplace.
* **Flexible:** Can be used for more than just CI/CD.
* **Generous free tier:** Great for students and open-source projects.

---

### Slide 11: Part 2: Core Concepts of GitHub Actions

**What we will cover:**

* Workflows
* Events
* Jobs
* Steps
* Actions
* Runners

---

### Workflows

* An automated process that you define in your repository.
* Defined by a YAML file in the `.github/workflows` directory.
* A repository can have multiple workflows.

---

### Anatomy of a Workflow File

```yaml
name: My First Workflow

on: [push]

jobs:
  my-job:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Hello, world!"
```

---

### Events

* A specific activity that triggers a workflow run.
* Examples: `push`, `pull_request`, `schedule`, `workflow_dispatch`.

---

### Slide 15: The `on` keyword

* Specifies the trigger for the workflow.
* Can be a single event, an array of events, or a map of events with configuration.

---

### Slide 16: Event Filtering

* You can filter events based on branches, tags, paths, etc.

```yaml
on:
  push:
    branches: [ main ]
    paths: [ '**.js' ]
```

---

### Jobs

* A set of steps that execute on the same runner.
* Each job runs in a fresh instance of a virtual environment.
* Jobs run in parallel by default.

---

### The `jobs` keyword

* Contains a list of jobs to run.
* Each job has a unique ID (e.g., `my-job`).

---

### Steps

* An individual task that can run commands or an action.
* Steps are executed in order within a job.
* Can have a `name` for better readability in the logs.

---

### The `steps` keyword

* Contains a list of steps for the job.
* `uses`: Specifies an action to run.
* `run`: Specifies a command to run on the runner's shell.

---

### Actions

* A reusable unit of code that can be used in a workflow.
* The building blocks of your workflows.
* You can create your own or use actions from the community.

---

### The `uses` keyword

* Tells the job to retrieve an action and run it.
* The action can be from the Marketplace, a public repo, or your own repo.
* It's recommended to pin to a specific version (e.g., `@v6`).

---

### Runners

* A server that has the GitHub Actions runner application installed.
* Listens for available jobs, runs one job at a time, and reports the progress, logs, and results back to GitHub.

---

### GitHub-Hosted Runners

* Provided by GitHub.
* Available in a variety of operating systems: `ubuntu-latest`, `windows-latest`, `macos-latest`.
* Clean, fresh environment for every job.

---

### Self-Hosted Runners

* You can host your own runners.
* Gives you more control over the hardware, operating system, and software tools.
* Useful for private networks or specific hardware requirements.

---

### Part 3: Lab 1: Your First Workflow

**Goal:** Create a simple "Hello World" workflow.

---

### Lab 1 Instructions (10 mins)

1. Create a new repository on GitHub.
2. In your repository, create a directory named `.github/workflows`.
3. Inside this directory, create a new file named `hello.yml`.

---

### Lab 1 Instructions (Continue)

4. Define a workflow that:
    * Triggers on a `push` event.
    * Has one job named `greeting_job`.
    * Runs on `ubuntu-latest`.
    * Has one step that prints "Hello, GitHub Actions!".
5. Commit and push your changes.
6. Go to the "Actions" tab of your repository to see the workflow run.

---

### Viewing Workflow Runs

* The "Actions" tab shows a list of all your workflow runs.
* You can click on a run to see the details of each job and step.

---

### Understanding the Logs

* Each step has its own section in the log.
* You can expand the sections to see the commands that were run and their output.

---

### Part 4: Workflows, Jobs, and Steps in Detail

**What we will cover:**

* Contexts and Expressions
* Dependencies between Jobs
* Job Strategies
* Artifacts

---

### Contexts

* A way to access information about the workflow run, runner, jobs, and steps.
* Available as objects that you can access using expression syntax.
* Example: `github.event_name`

---

### Common Contexts

* `github`: Information about the workflow run.
* `env`: Environment variables.
* `job`: Information about the current job.
* `steps`: Information about the steps in the current job.
* `runner`: Information about the runner.
* `secrets`: Secrets available to the workflow.

---

### Expressions

* Allow you to write dynamic code in your workflow files.
* Syntax: `${{ <expression> }}`
* Example: `if: github.event_name == 'push'`

---

### `if` conditionals

* You can use `if` conditionals to control whether a job or step is run.
* The expression is evaluated at the beginning of the job or step.

---

### Dependencies Between Jobs

* By default, jobs run in parallel.
* You can use the `needs` keyword to create dependencies between jobs.
* The dependent job will wait for the needed job to complete successfully.

---

### `needs` example

```yaml
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - run: echo "Building..."
  test:
    runs-on: ubuntu-latest
    needs: build
    steps:
      - run: echo "Testing..."
```

---

### Job Strategies

* Allows you to create multiple runs of a job based on a matrix of configurations.
* Useful for testing against different versions of a language, OS, etc.

---

### `strategy` and `matrix` example

```yaml
jobs:
  test:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [20.x, 21.x, 22.x]
    steps:
      - uses: actions/setup-node@v6
        with:
          node-version: ${{ matrix.node-version }}
      - run: npm test
```

---

### Slide 40: Artifacts

* Allow you to persist data between jobs in a workflow.
* Can also be used to save build outputs after a workflow has completed.

---

### `actions/upload-artifact`

* An action to upload files from your workflow run.

```yaml
- uses: actions/upload-artifact@v4
  with:
    name: my-artifact
    path: my-file.txt
```

---

### `actions/download-artifact`

* An action to download artifacts that were uploaded in a previous job.

```yaml
- uses: actions/download-artifact@v4
  with:
    name: my-artifact
```

---

### Artifact Retention

* Artifacts are stored for 90 days by default.
* You can configure a custom retention period.

---

### Caching Dependencies

* Workflows can be slow if they have to download dependencies every time.
* The `actions/cache` action allows you to cache dependencies to speed up your workflows.

---

### `actions/cache` example

```yaml
- uses: actions/cache@v5
  with:
    path: ~/.npm
    key: ${{ runner.os }}-node-${{ hashFiles('**/package-lock.json') }}
    restore-keys: |
      ${{ runner.os }}-node-
```

---

### Part 5: Lab 2: Building a Real CI Pipeline

**Goal:** Create a CI pipeline for a Node.js application.

---

### Lab 2 Instructions

1. Create a simple Node.js project with `package.json`, `index.js`, and `index.test.js`.
2. Add `jest` as a dev dependency.
3. Add a `test` script to `package.json`: `"test": "jest"`.

---

### Lab 2 Instructions (Continue)

4. Create a workflow that:
    * Triggers on `push` to `main` and on `pull_request`.
    * Has a `build` job that runs on `ubuntu-latest`.
    * Uses a matrix strategy to test on Node.js 20.x and 21.x.
    * Checks out the code.
    * Sets up the correct Node.js version.
    * Caches npm dependencies.
    * Installs dependencies (`npm ci`).
    * Runs the tests (`npm test`).

---

### Lab 2 Follow-up

* Observe the two parallel jobs created by the matrix strategy.
* Check the logs to see the cache being saved and restored.
* Create a pull request to see the workflow run.

---

### Part 6: Actions and Reusable Workflows

**What we will cover:**

* Types of Actions
* Using Actions from the Marketplace
* Creating your own Actions
* Reusable Workflows

---

### Types of Actions

* **Docker container actions:** Run in a Docker container, providing a consistent environment.
* **JavaScript actions:** Run directly on the runner machine. Faster than Docker actions.
* **Composite actions:** Combine multiple workflow steps into one action.

---

### `actions/checkout`

* The most common action.
* Checks out your repository so your workflow can access it.
* `@v6` is the latest major version.

---

### `actions/setup-*`

* A family of actions for setting up various language environments.
* Examples: `actions/setup-node`, `actions/setup-python`, `actions/setup-java`.

---

### Finding Actions

* The [GitHub Marketplace](https://github.com/marketplace?type=actions) is the best place to find actions.
* You can search by category or keyword.
* Look for actions that are well-maintained and have good reviews.

---

### Versioning Actions

* It's highly recommended to pin actions to a specific version.
* Using a major version (`@v6`) is a good balance of stability and getting new features.
* Using a specific commit SHA is the most secure and stable.

---

### Creating Your Own Actions

* You can create actions for your own use or to share with the community.
* An action is a repository with an `action.yml` metadata file.

---

### `action.yml` metadata file

* Defines the action's inputs, outputs, and how it runs.

```yaml
name: 'My Action'
description: 'A cool action'
inputs:
  who-to-greet:
    description: 'Who to greet'
    required: true
    default: 'World'
runs:
  using: 'node20'
  main: 'index.js'
```

---

### Creating a JavaScript Action

1. Create a new repository.
2. Add an `action.yml` file.
3. Write the JavaScript code for your action (e.g., in `index.js`).
4. Use the `@actions/core` and `@actions/github` packages to interact with the workflow.

---

### Creating a Composite Action

* A way to create an action by combining existing steps, without needing to write JavaScript or create a Docker image.

```yaml
# action.yml
name: 'My Composite Action'
runs:
  using: 'composite'
  steps:
    - run: echo "Hello ${{ inputs.who-to-greet }}"
      shell: bash
```

---

### Using Your Own Actions

* You can reference an action in the same repository: `uses: ./path/to/action`
* You can reference an action in another repository: `uses: owner/repo@ref`

---

### Reusable Workflows

* Allow you to call one workflow from another.
* Reduces duplication and makes your workflows easier to maintain.

---

### Creating a Reusable Workflow

* A reusable workflow is a normal workflow file that is called by another workflow.
* It must have a `workflow_call` trigger.

```yaml
# reusable-workflow.yml
on:
  workflow_call:
    inputs:
      username:
        required: true
        type: string
```

---

### Calling a Reusable Workflow

* Use the `uses` keyword in a job to call a reusable workflow.

```yaml
jobs:
  call-workflow:
    uses: owner/repo/.github/workflows/reusable-workflow.yml@main
    with:
      username: 'powerranger'
```

---

### When to Use Reusable Workflows

* When you have the same set of jobs in multiple workflows.
* To enforce a consistent CI/CD process across multiple repositories in an organization.

---

### Part 7: Lab 3: Using Marketplace Actions

**Goal:** Use a Marketplace action to add a comment to a pull request.

---

### Lab 3 Instructions

1. Create a new workflow that triggers on `pull_request`.
2. Find an action on the Marketplace that can add a comment to a pull request (e.g., `peter-evans/find-comment`).
3. Use this action to add a "Hello, PR!" comment to any new pull request.
4. You will need to use the `github.token` to give the action permission to comment.

---

### The `GITHUB_TOKEN`

* An automatically generated secret that is available in every workflow run.
* Allows you to authenticate to the GitHub API.
* Has limited permissions by default.

---

### Lab 3 Follow-up

* Create a new branch, make a change, and open a pull request.
* Verify that the action runs and adds a comment to your PR.

---

### Slide 71: Part 8: Secrets and Secure Workflows

**What we will cover:**
* What are secrets?
* How to use secrets in your workflows.
* Security best practices.

---

### What are Secrets?

* Encrypted environment variables that you can create in an organization, repository, or environment.
* For storing sensitive information like API keys, access tokens, etc.
* Once created, you can read their values, but you cannot edit them.

---

### Creating Secrets

* Repository secrets: `Settings > Secrets and variables > Actions`
* Organization secrets: `Organization Settings > Secrets and variables > Actions`
* Environments: `Settings > Environments`

---

### Using Secrets in a Workflow

* Access secrets using the `secrets` context.
* ` ${{ secrets.YOUR_SECRET_NAME }} `
* GitHub redacts secrets from the logs.

---

### `secrets` example

```yaml
steps:
  - name: Use my secret
    run: my-command --api-key ${{ secrets.MY_API_KEY }}
```

---

### Security Best Practices

* **Never log secrets:** GitHub tries to redact them, but you should not print them to the console intentionally.
* **Never store secrets in your repository code.**
* **Use the principle of least privilege:** Only give your workflows the permissions they need.

---

### Controlling Permissions for `GITHUB_TOKEN`

* You can set default permissions for the `GITHUB_TOKEN` at the organization or repository level.
* You can also specify permissions at the workflow level.

```yaml
permissions:
  contents: read
  pull-requests: write
```

---

### OpenID Connect (OIDC)

* A way to authenticate to cloud providers (like AWS, Azure, GCP) without needing to store long-lived secrets in GitHub.
* More secure than using static API keys.

---

### Environments

* A way to configure protection rules and secrets for a specific deployment target (e.g., `production`, `staging`).
* Can require a manual approval before a job that uses the environment can run.

---

### Dependency Review

* GitHub can automatically scan your pull requests for known vulnerabilities in your dependencies.
* Helps you catch security issues before they reach your codebase.

---

### Part 9: Lab 4: Working with Secrets

**Goal:** Use a secret to personalize a workflow.

---

### Lab 4 Instructions

1. Create a new repository secret named `GREETING_MESSAGE` with the value "Hello from a secret!".
2. Create a new workflow that triggers on `workflow_dispatch`.
3. The workflow should have one job that runs on `ubuntu-latest`.
4. The job should have a step that prints the value of the `GREETING_MESSAGE` secret.
5. Run the workflow manually from the Actions tab.

---

### `workflow_dispatch` trigger

* Allows you to run a workflow manually from the Actions tab.
* You can also define inputs that the user can provide when running the workflow.

---

### Lab 4 Follow-up

* Check the logs of the workflow run.
* You will see `***` instead of the secret's value. This is GitHub redacting the secret.
* To prove it's working, you could use the secret in a real command, e.g., passing it to another script or tool.

---

### Part 10: Advanced Topics and Best Practices

**What we will cover:**

* Deployment with GitHub Actions
* Creating custom Actions
* Organizing your workflows

---

### Deployment with GitHub Actions

* You can use GitHub Actions to deploy your application to any cloud provider or server.
* Use environments to protect your deployment targets.
* Use OIDC for secure authentication.

---

### Deployment to GitHub Pages

* A common use case for static sites.
* The `actions/deploy-pages` action makes this easy.

---

### Advanced Caching

* You can have more complex caching strategies.
* Use the `restore-keys` to use a less specific cache if an exact match is not found.

---

### Debugging Workflows

* Use `mxschmitt/action-tmate` to get a shell session on the runner for debugging.
* Enable step debug logging by creating a secret `ACTIONS_STEP_DEBUG` with the value `true`.

---

### Self-Hosted Runner Security

* Be careful when using self-hosted runners on public repositories.
* A malicious user could potentially run arbitrary code on your runner.
* It's recommended to only use self-hosted runners on private repositories.

---

### Organizing Your Workflows

* Use descriptive names for your workflow files.
* Use comments in your YAML files to explain complex parts.
* Use reusable workflows to reduce duplication.

---

### GitHub CLI and Actions

* The `gh` command-line tool can be used to interact with your workflows.
* `gh run list`: List recent runs.
* `gh run watch <run-id>`: Watch a run in real time.
* `gh workflow run`: Trigger a `workflow_dispatch` workflow.

---

### Run GitHub Actions Locally

**`https://nektosact.com/`**

---

### Keeping Actions Up-to-Date

* Use a tool like Dependabot to automatically create pull requests to update your actions to the latest versions.

---

### The Future of GitHub Actions

* GitHub is constantly adding new features.
* Stay up-to-date by reading the GitHub blog and changelog.

---

### Part 11: Conclusion and Q&A

**What we've covered:**

* The "why" and "what" of GitHub Actions.
* Core concepts and workflow syntax.
* Building CI pipelines.
* Using and creating actions.
* Security best practices.

---

### Summary and Key Takeaways

* GitHub Actions is a powerful automation tool.
* Start simple and build up complexity.
* Leverage the community via the Marketplace.
* Think about security from the start.

---

### Further Learning Resources

* [GitHub Skills](https://skills.github.com/)
* [GitHub Actions Documentation](https://docs.github.com/en/actions)
* [GitHub Blog - Actions Category](https://github.blog/category/actions/)
* [Awesome Actions](https://github.com/sdras/awesome-actions)

---

### Q&A / Thank You

**Thank you!**

---

### Lab 1 Solution

```yaml
# .github/workflows/hello.yml
name: Hello World Workflow

on: [push]

jobs:
  greeting_job:
    runs-on: ubuntu-latest
    steps:
      - name: Say Hello
        run: echo "Hello, GitHub Actions!"
```

---

### Lab 2 Solution ###

---

### **`package.json`**

```json
{
  "scripts": {
    "test": "jest"
  },
  "devDependencies": {
    "jest": "^29.0.0"
  }
}
```

---

### **`index.js`**

```javascript
function sum(a, b) { return a + b; }
module.exports = sum;
```

---

### **`index.test.js`**

```javascript
const sum = require('./index');
test('adds 1 + 2 to equal 3', () => {
  expect(sum(1, 2)).toBe(3);
});
```

---

### `ci.yml`

```yaml
# .github/workflows/ci.yml
name: Node.js CI

on:
  push:
    branches: [ main ]
  pull_request:

jobs:
  build:
    runs-on: ubuntu-latest
    strategy:
      matrix:
        node-version: [20.x, 21.x]
    steps:
    - uses: actions/checkout@v6
    - name: Use Node.js ${{ matrix.node-version }}
      uses: actions/setup-node@v6
      with:
        node-version: ${{ matrix.node-version }}
        cache: 'npm'
    - run: npm ci
    - run: npm test
```

---

### Lab 3 Solution

---

### `pr-comment.yml`

```yaml
# .github/workflows/pr-comment.yml
name: PR Commenter

on:
  pull_request:
    types: [opened]

jobs:
  comment:
    runs-on: ubuntu-latest
    steps:
      - name: Comment on new PR
        uses: actions/github-script@v8
        with:
          script: |
            github.rest.issues.createComment({
              issue_number: context.issue.number,
              owner: context.repo.owner,
              repo: context.repo.repo,
              body: 'Hello, PR!'
            })
```

*Note: Using `github-script` is often easier and more secure than third-party actions for simple API calls.*

---

### Lab 4 Solution

---

### `secrets.yml`

```yaml
# .github/workflows/secrets.yml
name: Secrets Demo

on:
  workflow_dispatch:

jobs:
  show-secret:
    runs-on: ubuntu-latest
    steps:
      - name: Echo the secret message
        run: echo ${{ secrets.GREETING_MESSAGE }}
```
