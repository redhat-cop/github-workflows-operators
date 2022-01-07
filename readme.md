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
* OPERATOR_SDK_VERSION - (Optional) Verison of Operator SDK for installation.
* BUILD_PLATFORMS - (Optional) Defaults to ```linux/amd64,linux/arm64,linux/ppc64le``` comma seperated list of ```os/arch``` build targets.
* PR_ACTOR - (Optional) Email address tied to Github Account for Pull Requests

#### Calling this Workflow - Job Example:
```
jobs:
  release-operator:
    name: release-operator
    uses: nickjordan/github-workflows-operators/.github/workflows/release-operator.yml@main
    secrets: 
      COMMUNITY_OPERATOR_PAT: ${{ secrets.COMMUNITY_OPERATOR_PAT }}
      BUNDLE_IMAGE_REPOSITORY: ${{ secrets.BUNDLE_IMAGE_REPOSITORY }}
      OPERATOR_IMAGE_REPOSITORY: ${{ secrets.OPERATOR_IMAGE_REPOSITORY }}
      REGISTRY_USERNAME: ${{ secrets.REGISTRY_USERNAME }}
      REGISTRY_PASSWORD: ${{ secrets.REGISTRY_PASSWORD }}
    with: 
      BUILD_PLATFORMS: "linux/amd64,linux/arm64,linux/ppc64le"
      PR_ACTOR: "user@tld.com
```



