# Extended Platform Tests

This repository holds the non-kubernetes, end-to-end tests that need to pass on a running
cluster before PRs merge and/or before we ship a release.
These tests are based on ginkgo and the github.com/kubernetes/kubernetes e2e test framework.

Prerequisites
-------------

* Git installed.
* Golang installed.
* Have the environment variable `KUBECONFIG` set pointing to your cluster.

## Compile the executable binary
The generated `extended-platform-tests` binary in the `cmd/extended-platform-tests/` folder.
If you want to compile the `openshift-tests` binary, please see the [origin](https://github.com/openshift/origin).

```console
$ mkdir -p ${GOPATH}/src/github.com/openshift/
$ cd ${GOPATH}/src/github.com/openshift/
$ git clone git@github.com:openshift/openshift-tests.git
$ make WHAT=cmd/extended-platform-tests  
```

Run `./extended-platform-tests --help` to get started.

```console
This command verifies behavior of an OpenShift cluster by running remote tests against the cluster API that exercise functionality. In general these tests may be disruptive or require elevated privileges - see the descriptions of each test suite.

Usage:
   [command]

Available Commands:
  help        Help about any command
  run         Run a test suite
  run-monitor Continuously verify the cluster is functional
  run-test    Run a single test by name
  run-upgrade Run an upgrade suite

Flags:
  -h, --help   help for this command
```

## How to run

You can filter your test case by using `grep`. Such as, 
For example, to filter the [OLM test cases](https://github.com/openshift/openshift-tests/blob/master/test/extended/operators/olm.go#L21), you can run this command: 

```console
$ ./extended-platform-tests run all --dry-run|grep "\[Feature:Platform\] OLM should"
I0410 15:33:38.465141    7508 test_context.go:419] Tolerating taints "node-role.kubernetes.io/master" when considering if nodes are ready
"[Feature:Platform] OLM should Implement packages API server and list packagemanifest info with namespace not NULL [Suite:openshift/conformance/parallel]"
"[Feature:Platform] OLM should [Serial] olm version should contain the source commit id [Suite:openshift/conformance/serial]"
"[Feature:Platform] OLM should be installed with catalogsources at version v1alpha1 [Suite:openshift/conformance/parallel]"
"[Feature:Platform] OLM should be installed with clusterserviceversions at version v1alpha1 [Suite:openshift/conformance/parallel]"
"[Feature:Platform] OLM should be installed with installplans at version v1alpha1 [Suite:openshift/conformance/parallel]"
"[Feature:Platform] OLM should be installed with operatorgroups at version v1 [Suite:openshift/conformance/parallel]"
"[Feature:Platform] OLM should be installed with packagemanifests at version v1 [Suite:openshift/conformance/parallel]"
"[Feature:Platform] OLM should be installed with subscriptions at version v1alpha1 [Suite:openshift/conformance/parallel]"
"[Feature:Platform] OLM should have imagePullPolicy:IfNotPresent on thier deployments [Suite:openshift/conformance/parallel]"
```

You can save the above output to a file and run it:

```console
$ ./extended-platform-tests run -f <your file path/name>
```

Or you can run it directly:

```console
$ ./extended-platform-tests run all --dry-run | grep "\[Feature:Platform\] OLM should" | ./extended-platform-tests run --junit-dir=./ -f -
```

## Run ISV Operators test

```console
$ ./extended-platform-tests run openshift/isv --dry-run | grep -E "<REGEX>" | ./extended-platform-tests run -f -
```
