# Top level Parent pom

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

## License

This project is licensed under the Apache License 2.0 - see the [LICENSE](LICENSE) file for details.

## Author

Dan Rollo
