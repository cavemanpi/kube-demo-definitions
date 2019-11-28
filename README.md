# kube-demo-definitions

This is a repo containing some simple definitions for Kubernetes object. It is meant to be in support of another presentation.

# Prerequisites

### OSX and nix systems

* Clone this repo.
* Install `kubectl` (https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-macos). The `kubectl` command line utility is the Swiss army knife of kubernetes cluster management. Don't worry about configuring it. When you start `minikube` below, minikube will configure kubectl.
  * I encountered a scenario when installing with homebrew, and then updating kubectl where it wasn't working correctly. Solution was to run
  ```
  rm /usr/local/bin/kubectl
  brew link --overwrite kubernetes-cli
  ```
* Install `virtualbox` (https://www.virtualbox.org/wiki/Downloads). This is a virtual machine hyperviser. This is not the default hypervisor for `minikube`. I encountered problems on OSX with the default hypervisor (Hyper-V) where hostname resolution wasn't working properly to pull down all the images kubernetes needs to run. Using `virtualbox` resolved this issue for me. This issue wasn't present on Windows. Untested on nix distros.
* Install `minikube` (https://kubernetes.io/docs/setup/learning-environment/minikube/) or otherwise have a kubernetes cluster available. Minikube is a virtual machine setup which gives you a local kubernetes learning environment.
  * You should start the VM at least once to make sure it initializes correctly.
  ```
  minikube start --cache-images=false --vm-driver virtualbox
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
  docker pull everpeace/curl-jq
  docker pull cavemanpi/toy-greeter-api:0.2
  docker pull cavemanpi/toy-greeter-ui:0.1
  ```
* (Optional) Install `watch`. This can be installed by your package management system including `brew` on mac. It is a command line utility which allows you to execute commands in a loop over a time interval. I find it helpful for visualizing changes to pods by running something like:
```
watch -n 1 kubectl get pods
```

### Windows 10

Note: You need Administrator access on your computer set up minikube following these instructions.

* Verify you have everything you need to run Hyper-V
  * Open a command prompt and run:
  ```
  systeminfo
  ```
  * If you see the following, you are set to continue, otherwise you may not be able to complete the rest of the setup:
  ```
  Hyper-V Requirements:     VM Monitor Mode Extensions: Yes
                              Virtualization Enabled In Firmware: Yes
                            Second Level Address Translation: Yes
                            Data Execution Prevention Available: Yes
  ```
* Clone this repo.
* Enable the Hyper-V hypervisor
  * Open a PowerShell console as Administrator.
  * Run the following command:
  ```
  Enable-WindowsOptionalFeature -Online -FeatureName Microsoft-Hyper-V -All
  ```
* Install `kubectl` (https://kubernetes.io/docs/tasks/tools/install-kubectl/#install-kubectl-on-windows) The `kubectl` command line utility is the Swiss army knife of kubernetes cluster management. Don't worry about configuring it. When you start `minikube` below, minikube will configure kubectl.
* Install `minikube` (https://kubernetes.io/docs/tasks/tools/install-minikube/) or otherwise have a kubernetes cluster available. Minikube is a virtual machine setup which gives you a local kubernetes learning environment.
  * The `chocolatey` installer didn't work for me. I ended up just using exe installer.
  * I highly recommend adding a shortcut to the minikube executable to your path. The default installation path for me was c:\Program Files\Kubernetes\Minikube\minikube
  * You should start the VM at least once to make sure it initializes correctly.
    * Open a command prompt as an administrator and run:
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
  docker pull everpeace/curl-jq
  docker pull cavemanpi/toy-greeter-api:0.2
  docker pull cavemanpi/toy-greeter-ui:0.1
  ```


# Challenge

For the challenge, I would like you to take the concepts you have learned in the presentation and use them to deploy a couple applications into your local minikube instance or other cluster. Above I asked you to pull down two images into your minikube VM, `cavemanpi/toy-greeter-api:0.2` and `cavemanpi/toy-greeter-ui:0.1`. Documentation for each of these is below. The requirements are:

1. Create an instance of the `cavemanpi/toy-greeter-api:0.2` application.
2. Create an instance of the `cavemanpi/toy-greeter-ui:0.1` application.
3. The `cavemanpi/toy-greeter-api:0.2` instance must be reachable by the instance of `cavemanpi/toy-greeter-ui:0.1`.
4. The `cavemanpi/toy-greeter-ui:0.1` is reachable from outside the cluster by your host machine's browser.
5. The default username and password should be overridden for the instances of both applications.

### Toy Greeter API
The toy greeter api, found at https://cloud.docker.com/u/cavemanpi/repository/docker/cavemanpi/toy-greeter-api is a simple flask application which returns a JSON dictionary containing a single key, `salutation`. The content of the `salutation` key is a string randomly selected from a small list of options.

For a request to the application to be successful, the requester must provide credentials via basic auth. The application can be configure to accept particular credentials by providing it with the following environment variables:

* USER - The username a user must connect with. Default: `default`
* PASSWORD - The password a user must connect with. Default: `default`

### Toy Greeter UI
The toy greeter ui, found at https://cloud.docker.com/repository/docker/cavemanpi/toy-greeter-ui, is a simple flask app meant to be used in tandem with the toy greeter api application. It performs a request to the configured host. It expects the response to have a JSON body having the form of a dictionary containing the key `salutation`. It takes the string in that key and prints "Hello, {salutation}!" in a response to an HTTP request.

In order to make the correct API requests, this application must be configured. It can be configured with the following environment variables:

* API_USER - The username with which to make api requests. Default: `default`
* API_PASSWORD - The password with which to make api requests. Default: `default`
* API_HOST - The base url to use for forming api requests. Default: `http://default`

### Solution
Please feel free to work together or chat about this in the session's slack channel. The solutions I came up with will be pushed to this repo after the conference in the branch `solutions`. A new directory will be in that branch named `challenge_solutions`.
