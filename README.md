# Helm Charts Repository

This repository hosts a collection of [Helm](https://helm.sh) charts for various applications. Helm is a package manager for Kubernetes, and these charts provide a way to deploy configurable, out-of-the-box applications onto a Kubernetes cluster.

The charts in this repository are maintained and versioned using GitHub Actions, which automatically packages and releases updated charts.

Please refer to the individual sections below for information on how to use, debug, and release the charts.

## Table of Contents

<!-- NOTE: To update doctoc please run `npx doctoc ./README.md --notitle` -->

<!-- START doctoc generated TOC please keep comment here to allow auto update -->
<!-- DON'T EDIT THIS SECTION, INSTEAD RE-RUN doctoc TO UPDATE -->

- [Usage](#usage)
  - [`Nodejs` Chart example](#nodejs-chart-example)
    - [Installing the Chart](#installing-the-chart)
    - [Uninstalling the Chart](#uninstalling-the-chart)
- [Debugging Charts Locally](#debugging-charts-locally)
  - [Example](#example)
- [Release Process](#release-process)

<!-- END doctoc generated TOC please keep comment here to allow auto update -->

## Usage

[Helm](https://helm.sh) must be installed to use the charts. Please refer to
Helm's [documentation](https://helm.sh/docs) to get started.

Once Helm has been set up correctly, add the repo as follows:

```bash
helm repo add silte https://charts.silte.fi
```

If you had already added this repo earlier, run `helm repo update` to retrieve
the latest versions of the packages. You can then run `helm search repo silte`
to see the charts.

### `Nodejs` Chart example

#### Installing the Chart

`helm install my-nodejs silte/nodejs`

#### Uninstalling the Chart

`helm delete my-nodejs`

## Debugging Charts Locally

To debug charts locally, you can use the `helm template` command. This command takes the chart and generates the defined templates with the provided values.

Here is a generic script that can be used from the `charts` directory:

```bash
helm template \
  --values <path-to-values-file> \
  --values <path-to-additional-values-file> \
  <chart-name> \
  --debug
```

In this command:

- `--values` flag is used to specify the values file. You can use this flag multiple times to provide multiple values files.
- `<chart-name>` is the name of the chart you want to debug.
- `--debug` flag is used to enable verbose output. It's useful for debugging purposes as it prints out the computed values and all executed templates.

This command will print out the rendered chart templates with the provided values to the console. You can inspect this output to debug your chart.

### Example

Here is an example of how to use the above command with specific values:

```bash
helm template \
  --values ../../my-application/helm/values.yaml \
  nodejs \
  --debug
```

In this example, we are debugging the `nodejs` chart with values file `values.yaml`. The `--debug` flag is used to print out the computed values and all executed templates.

Remember to replace `<path-to-values-file>`, `<path-to-additional-values-file>`, and `<chart-name>` with your actual values.

## Release Process

The release process for the Helm charts in this repository is automated using GitHub Actions. Here's a high-level overview of the steps involved:

1. **Update the chart version:** Before making a release, you need to update the version number in the `Chart.yaml` file of the chart you want to release. This version number should follow [semantic versioning](https://semver.org/).

2. **Commit and push your changes:** Make the necessary changes to the charts in the `charts/` directory. This could be a new chart, or updates to an existing chart. Remember to only update one chart at a time for each release. Once you're done, commit your changes and push to the repository.

3. **GitHub Actions Workflow:** The workflow defined in `.github/workflows/release.yml` is triggered on each push to the repository. This workflow is responsible for packaging the Helm charts and creating a new release.

4. **Packaging the Helm charts:** The workflow packages the updated chart in the `charts/` directory using the `helm package` command. This creates a `.tgz` file for the chart.

5. **Creating a new release:** Once the chart has been packaged, the workflow creates a new GitHub release and attaches the packaged chart as an asset to the release.

6. **Updating the Helm repository:** After the release is created, the Helm repository is updated to include the new release. This allows users to install the updated chart using `helm install`.

Please note that the actual steps may vary based on the specific configuration of the GitHub Actions workflow in your repository.
