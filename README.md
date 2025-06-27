# argocd-application-deploy

Deploy or validate an Argo CD application using Git or Helm sources. This CloudBees custom action authenticates to an Argo CD instance, creates or updates an application, and optionally supports dry-run mode for validation.

## Features

- Supports Git and Helm as sources  
- Optionally performs a dry-run (diff) instead of applying  
- Outputs Argo CD sync status and application UI link  
- Basic input validation and optional debug logging  

## Inputs

| Name             | Required | Description                                                                 |
|------------------|----------|-----------------------------------------------------------------------------|
| `serverurl`      | Yes      | Argo CD server URL (e.g., `https://argocd.example.com`)                     |
| `user`           | Yes      | Username for Argo CD login                                                  |
| `credential`     | Yes      | Password or API token for Argo CD login                                    |
| `projectname`    | Yes      | Name of the Argo CD project                                                |
| `applicationname`| Yes      | Name of the Argo CD application                                            |
| `repourl`        | Yes      | Git or Helm repository URL                                                 |
| `targetrevision` | Yes      | Git branch/tag or Helm chart version                                       |
| `repopath`       | No       | Path in the Git repository (required for Git sources)                      |
| `chartname`      | No       | Helm chart name (required for Helm sources)                                |
| `destnamespace`  | Yes      | Kubernetes namespace for deployment                                        |
| `deploytype`     | Yes      | Must be either `git` or `helm`                                             |
| `dryrun`         | No       | If `true`, performs a diff and exits (default: `false`)                    |
| `debug`          | No       | If `true`, enables verbose shell logging                                   |
| `poll_time`      | No       | Reserved for future use (default: `2`)                                     |

## Outputs

| Name              | Description                                                        |
|-------------------|--------------------------------------------------------------------|
| `sync_status`     | Returns `success` if the application was created or updated        |
| `application_url` | Direct URL to view the application in Argo CD UI                  |

## Example Usage (GitHub Action)

```yaml
- name: Deploy via Argo CD
  uses: ./argocd-application-deploy
  with:
    serverurl: https://argocd.example.com
    user: admin
    credential: ${{ secrets.ARGOCD_PASSWORD }}
    projectname: example-project
    applicationname: my-app
    repourl: https://github.com/myorg/myrepo.git
    targetrevision: main
    repopath: apps/my-app
    destnamespace: default
    deploytype: git
    dryrun: false
    debug: true
