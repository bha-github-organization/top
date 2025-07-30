# Top level Parent pom
[![CI Status](https://github.com/bha-github-organization/top/actions/workflows/publish-to-maven-central.yml/badge.svg)](https://github.com/bha-github-organization/top/actions)

[Documentation - (maven site)](https://bha-github-organization.github.io/top/)

A reusable parent pom for maven projects.

## Features

- Defines common project properties and dependencies

## CI/CD Setup

This project uses GitHub Actions for continuous integration and deployment:

### PR Test Workflow

The PR Test workflow runs automatically when a pull request is created or updated against the main branch. It ensures that all tests pass before the PR can be merged.

### Publish to Nexus Workflow

The Publish to Nexus workflow runs automatically when code is pushed to the main branch (which happens when a PR is merged). It builds the project, runs the tests, and if all tests pass, it publishes the JAR to a private Nexus Repository Manager.

#### Required Secrets

To use the Publish to Nexus workflow, you need to set up the following secrets in your GitHub repository:

- `NEXUS_USERNAME`: Username for Nexus authentication
- `NEXUS_PASSWORD`: Password for Nexus authentication

#### Nexus Repository Configuration

The Nexus repository URLs are configured in the `pom.xml` file. Update these URLs to point to your actual Nexus repositories:

```xml
<distributionManagement>
  <repository>
    <id>nexus-repository</id>
    <name>Nexus Release Repository</name>
    <url>https://nexus.example.com/repository/maven-releases/</url>
  </repository>
  <snapshotRepository>
    <id>nexus-repository</id>
    <name>Nexus Snapshot Repository</name>
    <url>https://nexus.example.com/repository/maven-snapshots/</url>
  </snapshotRepository>
</distributionManagement>
```

### Publish Site to GitHub Pages Workflow

The Publish Site to GitHub Pages workflow runs automatically when code is pushed to the main branch. It builds the Maven site and publishes it to GitHub Pages, making the documentation accessible via a web browser.

#### GitHub Pages Setup

To use this workflow, you need to:

1. Go to your repository settings
2. Navigate to "Pages" under "Code and automation"
3. Under "Build and deployment", select "GitHub Actions" as the source
4. The site will be published at `https://<username>.github.io/<repository>/` after the workflow runs successfully

The workflow uses GitHub's official actions for Pages deployment and doesn't require any additional secrets.

## Publishing to Maven Central

This project is configured to publish to Maven Central via the Sonatype OSSRH (OSS Repository Hosting) service.

### Prerequisites

1. **Sonatype OSSRH Account**: You need to have an account on Sonatype JIRA and create a ticket to request a new project namespace. Follow the [OSSRH Guide](https://central.sonatype.org/publish/publish-guide/) for details.

2. **GPG Key**: You need a GPG key to sign the artifacts. Create one using:
   ```bash
   gpg --gen-key
   ```

   Export your GPG key in ASCII armor format:
   ```bash
   gpg --armor --export-secret-keys YOUR_KEY_ID > private-key.asc
   ```

### GitHub Secrets Setup

To use the Publish to Maven Central workflow, set up the following secrets in your GitHub repository:

- `OSSRH_USERNAME`: Username for Sonatype OSSRH
- `OSSRH_PASSWORD`: Password for Sonatype OSSRH
- `GPG_PRIVATE_KEY`: Your GPG private key in ASCII armor format (the content of private-key.asc)
- `GPG_PASSPHRASE`: Passphrase for your GPG key

### Publishing Process

#### Snapshot Deployment

Snapshots are automatically deployed when code is pushed to the main branch.

#### Release Deployment

To create a release:

1. Go to the "Actions" tab in your GitHub repository
2. Select the "Publish to Maven Central" workflow
3. Click "Run workflow"
4. Enter the release version (e.g., 1.0.0) in the input field
5. Click "Run workflow" to start the release process

The workflow will:
1. Set the specified version
2. Build and test the project
3. Sign the artifacts with GPG
4. Deploy to Maven Central
5. Automatically release the staging repository if all checks pass

## TODO

- Add more common dependencies to dependency management section
- Create example child POM to demonstrate usage
- Add code quality plugins (e.g., SpotBugs, PMD, Checkstyle)
- Configure release plugin for automated releases
- Add more documentation on how to extend this parent POM
- Set up SonarQube integration for code quality analysis

## Author

Dan Rollo
