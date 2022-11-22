# Azure DevOps Pipeline Templates

This repository has [Azure DevOps Pipeline Templates](https://learn.microsoft.com/en-us/azure/devops/pipelines/process/templates) for performing various operations. 

You must add a repository reference to this repository in your **azure-pipelines.yml** before you can reference the teamplates contained within:

```yaml
resources:
  repositories:
    - repository: carlin
      type: github
      name: carlin-q-scott/azure-devops-pipeline-templates
      # ref: any git ref, such as commit hash to avoid changes to your pipeline
```

## Kube Control (kubectl)

This template allows you to manage your Azure Kubernetes Service cluster with Azure RBAC enabled.

example reference:
```yaml
parameters:
- name: azureSubscriptionEndpoint
  type: string
  default: the name of your Azure Resource Manager connection that you've granted admin access to your cluster

steps:
- template: /steps/kubectl.yaml@carlin
  parameters:
    displayName: Deploy to $(cluster) cluster
    azureSubscriptionEndpoint: ${{ parameters.azureSubscriptionEndpoint }}
    azureResourceGroup: $(resourceGroup)
    kubernetesCluster: $(cluster)
    inlineScript: |
      echo "Applying manifests to $(cluster) cluster"
      kubectl apply -k manifests/$(Environment.Name)
```
