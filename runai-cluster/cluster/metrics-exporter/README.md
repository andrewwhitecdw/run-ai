# Background:
A service that watches for changes in cluster resources (Pods, Queues, Nodes, etc.) 
and exports the relevant metrics for that updated resource.


# Usage:
## Build
to build the service run:

``make build``

The result binary will be located in `bin` folder.

## Test
To run all the tests under `/pkg`, run:

``make test``

## Run/Debug via IDE
To run or debug locally, just add an environment variable `KUBECONFIG` that points to your kubeconfig file and execute /cmd/main.go

