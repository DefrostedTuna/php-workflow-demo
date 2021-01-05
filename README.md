# PHP Workflow Demo

This is simply a project to test out some new workflow practices.

The branching strategy used here will follow the Gitflow standard where there is a long lived `master` and `develop` branch. Feature branches are merged to `develop` and when `develop` is ready for a release, the current state of `develop` will be merged down to `master` where a release will take place in the form of a `tag`. 

This release will be automated via CI/CD and will be governed by [Conventional Commits](https://www.conventionalcommits.org/en/v1.0.0/) and [Standard Version](https://github.com/conventional-changelog/standard-version). Conventional Commits will be set up to adhere to a specific standard, which will be used to help generate proper version numbers and changelogs. Standard version will hook into the conventions put in place by Conventional Commits and will take advantage of CI/CD to perform the release and generate the changelog contents.

Builds will also be done for the `develop` branch in a similar fashion. Development builds will be versioned based on the commit hash of the most recent commit to the `develop` branch, rather than based on [SemVer](https://semver.org/). These development releases will not have a Git tag associated with them and will only contain a Docker image within the Docker Registry.

Deployments will take place automatically via CI/CD as well and will be staged on a Kubernetes cluster as a Docker image. As previously stated, commits pushed to the `develop` branch will automatically have Docker images built and pushed to the Docker registry with the tag being governed by the commit hash. When these images are pushed, they will be automatically detected and deployed to Kubernetes. 

Likewise, when a commit is pushed to `master`, the CI/CD pipeline will build a Docker image using the commit hash, while also using Standard Version to generate a Git tag and changelog. When the Git tag created by Standard Version is pushed to source control, it will have a Docker image built using the Git tag as the Docker tag. This tagged release will follow SemVer and will then be deployed to the Kubernetes cluster if it fits the schema defined in the cluster's config.