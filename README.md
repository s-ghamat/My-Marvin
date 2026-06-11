# My Marvin

Jenkins Configuration as Code project for building a reproducible CI/CD automation instance.

The project uses Jenkins LTS, JCasC, and Job DSL to define users, roles, permissions, folders, and jobs entirely as code.

## Overview

My Marvin is a Jenkins automation project focused on reproducibility and deterministic configuration.

The Jenkins instance is evaluated through automated tests. All configuration must therefore be declarative, centralized, and version-controlled.

## Principles

* Jenkins LTS only
* Minimal plugin set
* Configuration as Code
* Job definitions as code
* No manual UI configuration
* Deterministic and testable setup

## Required Files

The Jenkins configuration is defined with two files:

```text
.
├── my_marvin.yml
└── job_dsl.groovy
```

No additional JCasC or DSL files should be required.

## Plugins

Only the required plugins should be installed:

```text
cloudbees-folder
configuration-as-code
credentials
github
instance-identity
job-dsl
script-security
structs
role-strategy
ws-cleanup
```

## JCasC Configuration

The `my_marvin.yml` file defines the Jenkins instance configuration.

It includes:

* Global Jenkins configuration
* System message
* Disabled user sign-up
* Required users
* Role-based authorization strategy
* Role assignments
* Permissions expected by the project tests

System message:

```text
Welcome to the Chocolatine-Powered Marvin Jenkins Instance.
```

## Users

The following users must be created:

```text
Hugo
Garance
Jeremy
Nassim
```

Passwords must be provided through environment variables and must not be hardcoded.

Example:

```bash
export USER_HUGO_PASSWORD="..."
export USER_GARANCE_PASSWORD="..."
export USER_JEREMY_PASSWORD="..."
export USER_NASSIM_PASSWORD="..."
```

## Roles

The authorization strategy uses Role-Based Authorization Strategy.

Required roles:

```text
admin
ape
gorilla
assist
```

Each role must include the exact permissions expected by the automated tests.

## Job DSL

The `job_dsl.groovy` file defines the Jenkins jobs and folders.

Required elements:

* `Tools` folder
* `clone-repository` job
* `SEED` job
* Dynamically generated jobs from the seed job

## Jobs

### Tools Folder

Contains utility jobs used by the Jenkins instance.

### clone-repository

Parameterized job that clones a Git repository from a provided URL.

### SEED

Job DSL seed job used to generate project jobs dynamically.

Generated jobs must run the following shell steps in order:

```bash
make fclean
make
make tests_run
make clean
```

## Setup

### 1. Install Jenkins LTS

Install and run Jenkins using the current LTS version.

### 2. Install Required Plugins

Install only the required plugin set listed in this README.

### 3. Configure JCasC

Place `my_marvin.yml` at the root of the Jenkins configuration directory.

Set the JCasC path:

```bash
export CASC_JENKINS_CONFIG=/path/to/my_marvin.yml
```

### 4. Add Job DSL

Place `job_dsl.groovy` at the same root level as `my_marvin.yml`.

### 5. Set User Passwords

Export the required password variables before starting Jenkins:

```bash
export USER_HUGO_PASSWORD="..."
export USER_GARANCE_PASSWORD="..."
export USER_JEREMY_PASSWORD="..."
export USER_NASSIM_PASSWORD="..."
```

## Test Expectations

The automated tests validate:

* Jenkins runs on the LTS line
* Only required plugins are installed
* `my_marvin.yml` exists
* `job_dsl.groovy` exists
* Users are defined correctly
* Passwords come from environment variables
* Role-based authorization is configured
* Required roles and permissions exist
* Required jobs and folders are generated
* Generated jobs run the expected build steps

## Notes

This project is strict by design.

Manual changes made through the Jenkins UI are not considered reliable and may break reproducibility. All configuration should remain declarative, idempotent, and centralized in the required files.

## Academic Context

Project: My Marvin
Focus: Jenkins, CI/CD, Configuration as Code, Job DSL
School: Epitech

## Author

Setayesh Ghamat
