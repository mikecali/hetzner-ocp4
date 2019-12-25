# OpenShift Pipeline to test hetzner-ocp4

## Build ansible ubi

```
podman build -t quay.io/rbo/sandbox:ubi-ansible .
podman push quay.io/rbo/sandbox:ubi-ansible

```

## Create secret
```
oc create secret generic hetzner-ocp4-pipeline\
  --from-file=ssh-privatekey=/Users/rbohne/.ssh/pipeline \
  --from-file=gcp_service_account.json=/Users/rbohne/Desktop/rbohne-openshift-ssa-project-1-92c77d04054a.json \
  --from-file=cluster.yml=pipeline/cluster.yml \
  --from-file=cluster-test-01.yml=pipeline/cluster-test-01.yml \
  --from-file=cluster-test-02.yml=pipeline/cluster-test-02.yml   
  --from-file=cluster-test-02.yml=pipeline/cluster-test-03.yml   
    
```

## Install pipeline

```
oc create -f task-reset-hetzner.yaml
oc create -f - <<EOF
apiVersion: tekton.dev/v1alpha1
kind: PipelineResource
metadata:
  name: hetzner-ocp4
spec:
  type: git
  params:
    - name: url
      value: https://github.com/rbo/hetzner-ocp4.git
    - name: revision
      value: pipeline
EOF
oc create -f pipeline-hetzner-ocp4.yaml
```