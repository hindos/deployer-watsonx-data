# Techzone Deployer IBM watsonx.data pipeline

This repository contains a Tekton pipeline to deploy IBM watsonx.data onto an IBM Technology Zone `deployer` cluster.

## Pre-requisites

A `deployer` cluster is configured with the following items:

- ExternalSecrets operator deployed with a ClusterSecretStore configured. The remote ExternalSecrets secret store must include an IBM Entitlement Key.
- Techzone Deployer Tekton tasks deployed ([deploy YAML](https://github.com/cloud-native-toolkit/deployer-tekton-tasks/blob/main/argocd.yaml)).
- OpenShift GitOps configured with [One Touch Provisioning ArgoCD instance](https://github.com/one-touch-provisioning/otp-gitops), and any relevant RBAC rules.
- OpenShift Pipelines operator deployed.

For more information on the cluster configuration and deployment, see https://github.ibm.com/dte2-0/ccp-gitops-patterns/tree/test/vmware-openshift-ipi-deployer. While this pattern is a Terraform pattern, you can also use the `scripts/deploy.sh` file to configure your existing OpenShift cluster with OpenShift Data Foundation to configure dependencies.

## Deploying the pipelinerun

### Deploying with ArgoCD

These commands deploy an ArgoCD application, which sets up the tasks, pipeline and create a pipelinerun.

```shell
git clone github.com/cloud-native-toolkit/deployer-watsonxdata.git
cd deployer-watsonxdata
oc apply -f .
```

Wait. This total deployment can take 1hr30min.

### Deploying manually

By default, pipelines are run out of the `default` namespace.

#### Create task and pipeline

Apply the `ibm-lakehouse-manage.yaml` task and `pipeline.yaml` pipeline in the `default` namespace.

```shell
oc apply -f tasks/ibm-lakehouse-manage.yaml -n default
oc apply -f pipelines/pipeline.yaml -n default
```

#### Create pipelinerun

Start the pipelinerun and wait

```shell
oc create -f pipelines/pipelinerun.yaml -n default
```

Wait. This total process can take about 1hr30min.