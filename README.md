# Hello World Operator - Functionality demo for developers 

### Running locally 
* operator-sdk cli `operator-sdk up local`
* standard go tooling `go run cmd/manager/main.go` 
  ```bash
      export KUBERNETES_CONFIG=/Users/dima/.kube/config 
      export WATCH_NAMESPACE=hwop 
      export OPERATOR_NAME=hw 
  ``` 
  
### Reconciliation loop basic overview
* apply CR `oc apply -f deploy/crds/cr0.yaml`
* apply CR `oc apply -f deploy/crds/cr1.yaml`
* apply CR `oc apply -f deploy/crds/cr3.yaml`
* apply CR `oc apply -f deploy/crds/cr4.yaml`

### Watches 
* edit service when watching service changes 
* edit service when not watching service changes 

### Change the state 
* edit service, add label
* edit deployment, add label

### Delete the CR
* Delete CR when operator is running `oc delete -f deploy/crds/cr0.yaml`
* Delete CR when operator is not running `oc delete -f deploy/crds/cr0.yaml`

### Build Operator  
* with OperatorSDK `operator-sdk build <docker-image-name>`
* with standard go tools `go build -o hw-operator cmd/manager/main.go`
* with local S2I `s2i build . docker.io/dimssss/golang-s2i:0.5 test`
* with OpenShift SourceStrategy build 
