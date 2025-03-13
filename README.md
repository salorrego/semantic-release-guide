# Semantic Release Guide

This repository provides a comprehensive guide to using Semantic Release for automated versioning, as well as an example of how to configure it using [Github Actions](https://github.com/features/actions).

## What is Semantic Release?

Semantic Release automates the entire package release workflow, including:

- **Determining the next version number:** Based on your commit messages, adhering to semantic versioning.
- **Generating release notes:** Automatically creating detailed changelogs.
- **Publishing the package:** To your chosen registry (e.g., npm, PyPI, GitHub Releases). `Not included in this guide.`

This eliminates the manual, error-prone process of versioning and releasing software.

## Why Use Semantic Release?

- **Automation:** Frees up developer time and reduces manual errors.
- **Consistency:** Ensures consistent and predictable releases.
- **Semantic Versioning:** Enforces adherence to semantic versioning standards.
- **Improved Collaboration:** Provides clear and automated release notes.

## Table of Contents

- [Prerequisites](#prerequisites)
- [Installation](#installation)
- [Configuration](#configuration)
  - [Plugins](#plugins)
  - [Commit Message Conventions](#commit-message-conventions)
- [Continuous Integration (CI) Setup](#continuous-integration-ci-setup)
  - [GitHub Actions](#github-actions)

## Prerequisites

- A GitHub repository.

- Node.js installed.

- A basic understanding of Git and npm.

- A CI/CD pipeline (GitHub Actions) configured.

## Installation

Semantic Release CLI can be installed using `npm`, for this example we will be using `npm@10.9.2`.

```bash
# Example for npm:
npm install --save-dev semantic-release
```

## Configuration

### Commit Message Conventions

Semantic release is configued to run based on the commit message pushed to the selected branch, for our case we will be using `main` branch.

It uses [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) for the commit message in order to understand the type of change, and, based on the commit generate therespective version, `Mayor`, `Minor`, `Fix`.

For our case we will be ussing [commitizen](https://www.npmjs.com/package/commitizen) which is a CLI that helps us set up the commit easier by running a command and following a couple of steps.

- Install Commitizen and cz-conventional-changelog for commitizen config:
  > ```bash
  > npm install --save-dev commitizen cz-conventional-changelog
  > ```
- Usage of commitizen
  > You can either use `npx cz` to start the CLI
  > or you can create a script for it on `package.json` to use it as `npm run commit`
  >
  > ```
  > ...
  > "scripts": {
  >   "commit": "cz"
  > }
  > ...
  >
  > ```
- Add `.czrc` file for Commitizen config
  > ```
  > {
  >  "path": "cz-conventional-changelog"
  > }
  > ```

### Plugins

We will be using different plugins for our configuration since each one of them are gonna be executing something different.

Install next plugins for semantic-release:

```bash
npm install --save-dev @semantic-release/changelog @semantic-release/git @semantic-release/github @semantic-release/release-notes-generator
```

- @semantic-release/changelog
  > This plugin is used to generate a `CHANGELOG.md` based on the commits pushed to the specified branch.

In order to be able to use Semantic release and it's capabilities you will have to install different plugins that will be explained in detail on next section.

Install next plugins:

Then once you have dependencies installed you will have to create a `.releaserc.json` file. This will be used as configuration for your semantic release execution.

Content for `.releaserc.json`:

```
{
  "branches": ["main"],
  "plugins": [
    "@semantic-release/commit-analyzer",
    "@semantic-release/release-notes-generator",
    "@semantic-release/changelog",
    "@semantic-release/github",
    ["@semantic-release/git", {
      "assets": ["CHANGELOG.md", "package.json"],
      "message": "chore(release): ${nextRelease.version} [skip ci]\n\n${nextRelease.notes}"
    }]
  ]
}
```

## Continuous Integration (CI) Setup

For our CI we will be using [Github Actions](https://github.com/features/actions), but feel free to use your preferred CI, just remember to add the required permissions and tokens.

### GitHub Actions
