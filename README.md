# Save a LocalStack Cloud Pod

[![Test LocalStack Cloud Pod Save](https://github.com/HarshCasper/cloud-pod-save/actions/workflows/ci.yml/badge.svg)](https://github.com/HarshCasper/cloud-pod-save/actions/workflows/ci.yml)
[![tag badge](https://img.shields.io/github/v/tag/HarshCasper/cloud-pod-save)](https://github.com/HarshCasper/cloud-pod-save/tags)
[![license badge](https://img.shields.io/github/license/HarshCasper/cloud-pod-save)](./LICENSE)

A GitHub Action to save your local AWS infrastructure as a [Cloud Pod](https://docs.localstack.cloud/user-guide/tools/cloud-pods/) via your workflow. Users can save their Cloud Pod on their GitHub Actions runner, and save it as an Artifact or a Release Asset, or on the LocalStack Platform, which requires a [LocalStack Team](https://localstack.cloud/solutions/team-collaboration/) account.

## Usage

To get started, you can use this minimal example:

```yaml
- name: Run AWS commands
  run: |
    awslocal s3 mb s3://test
    awslocal sqs create-queue --queue-name test-queue
    awslocal sns create-topic --name test-topic

- name: Save the Cloud Pod 
  uses: HarshCasper/cloud-pod-save@v0.1.0
  with:
    name: localstack-cloud-pod
    location: disk
```

This will create a Cloud Pod named `localstack-cloud-pod` and save it on the runner's disk. You can then use [`actions/upload-artifact`](https://github.com/actions/upload-artifact) or an equivalent action to save the Cloud Pod as an Artifact.

To save the LocalStack Cloud Pod on the LocalStack Platform, you need to use [LocalStack GitHub Actions Login
](https://github.com/localstack/localstack-github-actions-login) action. You can then use the following example:

```yaml
- name: 🤔 Login to LocalStack
  uses: LocalStack/localstack-github-actions-login@v0.1.0
  with:
    email: ${{ secrets.LOCALSTACK_USERNAME }}
    password: ${{ secrets.LOCALSTACK_PASSWORD }}

- name: Run AWS commands
  run: |
    awslocal s3 mb s3://test
    awslocal sqs create-queue --queue-name test-queue
    awslocal sns create-topic --name test-topic

- name: Save the Cloud Pod 
  uses: HarshCasper/cloud-pod-save@v0.1.0
  with:
    name: localstack-cloud-pod
    location: platform
```

For a more detailed example, you can check out the [example workflow](./.github/workflows/ci.yml).

### Inputs

| Input       | Description                                                                     | Default     |
| ----------- | ------------------------------------------------------------------------------- | ----------- |
| `name`      | Name of the Cloud Pod                                                           | `cloud-pod` |
| `directory` | Name of the specific directory you want to save the Cloud Pod in                | None        |
| `location`  | Name of the specific location (disk/platform) you want to save the Cloud Pod in | `disk`      |

## License

[Apache License 2.0](./LICENSE)
