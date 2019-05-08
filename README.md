# managed-cluster-config repository

This repo contains static configuration specific to a "managed" OpenShift Dedicated (OSD) cluster.

## Prometheus

A set of rules and alerts that SRE requires to ensure a cluster is functioning.  There are two categories of rules and alerts found here:

1. SRE specific, will never be part of OCP
2. Temporary addition until made part of OCP

## SRE Authorization
Instead of SRE having `cluster-admin` a new ClusterRole is created with some permissions removed.  The ClusterRole can be regenerated in the `generate/sre-authorization` directory.


## Generating selectorsyncset 
To generate a selectorsyncset you must have a template file like that example below and the generation script found in `scripts/generate_syncset.py.`


###Example-Template
```yaml
apiVersion: hive.openshift.io/v1alpha1
kind: SelectorSyncSet
metadata:
  generation: 1
  name: {repo-name}-config
spec:
  clusterDeploymentSelector:
    matchLabels:
      api.openshift.com/managed: "true"
  resourceApplyMode: sync
```

Finally in the Makefile include a call to the script containing the path to the template and selectorselectorsync set destination, the directory where your yaml files are located and the git commit hash. In this repo those definitions are included in the `project.mk` file
