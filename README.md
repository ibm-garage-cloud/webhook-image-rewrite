# Kubernetes Mutating Webhook to Rewrite image paths

Defines a mutating webhook to rewrite images and replaces them with the configured mirror
 [MutatingAdmissionWebhook](https://kubernetes.io/docs/admin/admission-controllers/#mutatingadmissionwebhook-beta-in-19) 

## Prerequisites

- [git](https://git-scm.com/downloads)
- [go](https://golang.org/dl/) version v1.12+
- [docker](https://docs.docker.com/install/) version 17.03+
- [kubectl](https://kubernetes.io/docs/tasks/tools/install-kubectl/) version v1.11.3+
- Access to a Kubernetes v1.11.3+ cluster with the `admissionregistration.k8s.io/v1beta1` API enabled. Verify that by the following command:

```shell script
kubectl api-versions | grep admissionregistration.k8s.io
```

The result should be:

```
admissionregistration.k8s.io/v1
admissionregistration.k8s.io/v1beta1
```

> Note: In addition, the `MutatingAdmissionWebhook` and `ValidatingAdmissionWebhook` admission controllers should be added and listed in the correct order in the admission-control flag of kube-apiserver.

## Build

**Note:** The following commands use the IMAGE_REPO and IMAGE_TAG variables defined in the Makefile. These can be overridden 
at build time by either setting an environment variable in the shell or setting the value in the command. E.g.

```shell script
export IMAGE_TAG=0.1.0
export IMAGE_REPO=docker.io/mynamespace
make build-image push-image
``` 

- OR -

```shell script
IMAGE_TAG=0.1.0 IMAGE_REPO=docker.io/mynamespace make build-image push-image
``` 

1. Build binary

```shell script
make build
```

2. Build docker image
   
```shell script
make build-image
```

3. push docker image

```shell script
make push-image
```

> Note: log into the docker registry before pushing the image.

## Setup the CA bundle and the deployment

**Note:** The following command uses the IMAGE_REPO and IMAGE_TAG variables defined in the Makefile. These can be overridden 
at build time by either setting an environment variable in the shell or setting the value in the command. E.g.

```shell script
export IMAGE_TAG=0.1.0
export IMAGE_REPO=docker.io/mynamespace
make build-image push-image
``` 

- OR -

```shell script
IMAGE_TAG=0.1.0 IMAGE_REPO=docker.io/mynamespace make build-image push-image
``` 

```shell script
make setup
```

## Deploy

1. Create namespace `image-rewrite` in which the image rewrite webhook is deployed:

```shell script
kubectl create ns image-rewrite
```

2. Deploy resources:

```shell script
make deploy
```
