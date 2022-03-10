# Github Workflows for Operator Development
## About This Repository
This repoisitory contains work-in-progress example reusable Github Actions Workflows for Opeartor deployment and git-ops release to OLM (Operator Lifecycle Manager) Community Operators.
## Workflows 
### Release Operator 
#### About This  Workflow 
This workflow allows a caller repositiory following a common structure to build the operator, bundle, helm charts, and release to OpeatorHub via Github Fork PR request process.
#### Requirements 
* Fork of ```redhat-openshift-ecosystem/community-operators-prod``` created by ```PR_ACTOR``` for PR release process.
* Github branch ```gh-pages``` on caller-repo for Github Pages Helm Chart Hosting
##### Caller (Operator Repo) Configuration 
* Operator Dockerfile located at ```./Dockerfile``` in root of Repository
* Bundle Dockerfile located at ```./bundle.Dockerfile``` in root of Repository
##### Caller (Operator Repo) Github Repository Secrets
* COMMUNITY_OPERATOR_PAT - (Required) 'Github PAT Token for Community Operator Fork Git Operations'
* REGISTRY_USERNAME - (Required) Username for registry
* REGISTRY_PASSWORD - (Required) Password for registry
* OPERATOR_IMAGE_REPOSITORY - (Optonal) 'Registry image e.g. quay.io/redhat-cop/repository-name' will default to ```quay.io/github.repo.owner/repo.name```
* BUNDLE_IMAGE_REPOSITORY - (Optional) 'Registry image e.g. quay.io/redhat-cop/repository-name' will default to ```quay.io/github.repo.owner/repo.name```

##### Caller (Operator Repo) Workflow Inputs
* OPERATOR_SDK_VERSION - (Optional string) Verison of Operator SDK for installation.
* BUILD_PLATFORMS - (Optional string) Defaults to ```linux/amd64,linux/arm64,linux/ppc64le``` comma seperated list of ```os/arch``` build targets.
* PR_ACTOR - (Optional string) Email address tied to Github Account for Pull Requests
* RUN_UNIT_TESTS - (Optional boolean) Defaults to ```false```. If true, runs unit tests in the test-operator step by running the *test* target in the Makefile.
* RUN_INTEGRATION_TESTS - (Optional boolean) Defaults to ```false```. If true, runs integration tests in the test-operator step by running the *integration* target in the Makefile. If both `RUN_UNIT_TESTS` and `RUN_INTEGRATION_TESTS` are true, the unit tests will run first.

#### Calling this Workflow - Job Examples:

Example PR workflow in your ooperator's `.gihub/workflows/pr-operator.yml` file...

```yaml 
name: pull request
on:
  pull_request:
    branches:
      - master
      - main

jobs:
  shared-operator-workflow:
    name: shared-operator-workflow
    uses: trevorbox/github-workflows-operators/.github/workflows/pr-operator.yml@main
    with: 
      RUN_UNIT_TESTS: "true"
      RUN_INTEGRATION_TESTS: "true"
```

Example release workflow in your operator's `.gihub/workflows/release-operator.yml` file...

```yaml
name: push
on:
  push:
    branches:
      - main
      - master
    tags:
      - v*

jobs:
  shared-operator-workflow:
    name: shared-operator-workflow
    uses: trevorbox/github-workflows-operators/.github/workflows/release-operator.yml@main    
    secrets:
      COMMUNITY_OPERATOR_PAT: ${{ secrets.COMMUNITY_OPERATOR_PAT }}
      REGISTRY_USERNAME: ${{ secrets.REGISTRY_USERNAME }}
      REGISTRY_PASSWORD: ${{ secrets.REGISTRY_PASSWORD }}
    with:
      PR_ACTOR: "raffaele.spazzoli@gmail.com"
      RUN_UNIT_TESTS: "true"
      RUN_INTEGRATION_TESTS: "true"
```
