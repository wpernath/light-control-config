# light-control-config
This is the gitops config repo for the project [light-control](https://github.com/wpernath/light-control). 

## Prerequisites
It is currently only tested for Red Hat OpenShift, which you could get either 

- [CodeReady Containers](https://github.com/code-ready/crc) or
- [Single Node OpenShift (SNO)](https://console.redhat.com/openshift/create/datacenter) 

To use the content in this repository, you need to have setup your OpenShift Cluster to use the following Operators:

- OpenShift Pipelines (or Tekton)
- OpenShift GitOps (or argocd)
- Crunchy Data PostgreSQL Cluster

## Installation
### CI environment (tekton)
To install the CI environment, simply log into your OpenShift cluster and execute the following script

```shell-script
$ oc login -u <user> -p <pass> https://api.crc.testing:6443
$ ./tekton/pipeline.sh init --git-user <git user> \
                            --git-password <git-pwd> \ 
                            --repo-user <image repo user> \
                            --repo-password <image repo pwd> 
```
This will - behind the scenes - do a 
```shell-script
$ oc apply -k tekton/
```

And will then apply the secrets in the lights-ci namespace with your user/password combinations to access GitHub and Quay.io. If you do use other repositories, please feel free to edit the secrets accordingly.

### Dev and Stage projects
To install the ArgoCD apps which will then install the current versions for Dev and Stage, please issue the following command:

```shell-script
$ oc apply -k argocd
```

After this call, you have a `light-dev` and a `light-stage` namespace properly setup with the current config you find in the `config` folder.

## Development
