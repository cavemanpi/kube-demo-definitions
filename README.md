# kube-demo-definitions

This is a repo containing some simple definitions for Kubernetes object. It is meant to be in support of another presentation.

# Prerequisites

* Clone this repo.
* Install `kubectl` (https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos). The `kubectl` command line utility is the Swiss army knife of kubernetes cluster management.
  * I encountered a scenario when installing with homebrew, and then updating kubectl where it wasn't working correctly. Solution was to run
  ```
  rm /usr/local/bin/kubectl
  brew link --overwrite kubernetes-cli
  ```
* Install `minikube` (https://kubernetes.io/docs/setup/learning-environment/minikube/) or otherwise have a kubernetes cluster available. Minikube is a virtual machine setup which gives you a local kubernetes learning environment.
  * You should start the VM at least once to make sure it initializes correctly.
  * If starting minikube hangs when pulling the VM images, start with
  ```
  minikube start --cache-images=false
  ```
* Configure minikube to be able to use ingresses:
```
minikube addons enable ingress
```
* Log into the minikube VM and pull down some docker images. You ordinarily don't need to do this. Kubernetes typically pulls docker images for you. Doing this ahead of time will help us keep strain off the conference network and help the demo move faster.
  * Run `minikube ssh` to get a shell into the minikube VM.
  * Run
  ```
  docker pull nginx:1.14.2
  docker pull nginx:1.17.1
  docker pull python:3
  docker pull cavemanpi/toy-greeter-api:0.2
  docker pull cavemanpi/toy-greeter-ui:0.1
  ```
* Optional - Install `watch`. It is a command line utility which allows you to execute commands in a loop over a time interval. I find it helpful for visualizing changes to pods by running something like:
```
watch -n 1 kubectl get pods
```
