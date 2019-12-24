# OpenShift Pipeline to test hetzner-ocp4

## Build ansible ubi

```
podman build -t quay.io/rbo/sandbox:ubi-ansible .
podman push quay.io/rbo/sandbox:ubi-ansible

```

## Create secrets
```
$ oc create secret generic cluster-basic --from-file=cluster.yml=cluster-ci.yml
secret/cluster-basic created

$ oc create secret generic hetzner-ssh-key\
    --from-file=ssh-privatekey=/Users/rbohne/.ssh/pipeline \
    --type=kubernetes.io/ssh-auth
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