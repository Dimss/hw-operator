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
* `oc apply -f deploy/crds/cr0.yaml`
* `oc apply -f deploy/crds/cr1.yaml`
* `oc apply -f deploy/crds/cr2.yaml`
* `oc apply -f deploy/crds/cr3.yaml`

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
    ```bash
    # Create BC
    oc process -f deploy/bc.yaml | oc create -f -
    oc start-build hw-operator -F 
    ```
### Deploy Operator
* `oc create -f deploy/service_account.yaml` 
* `oc create -f deploy/role.yaml` 
* `oc create -f deploy/role_binding.yaml` 
* `oc create -f deploy/operator.yaml` 

## Part 1.1

 
#### Export variables
```bash
export OPERATOR_NAME=SET_YOUR_OPERATOR_NAME
export GROUP_NAME=SET_YOUR_OPERATOR_GROUP
export OPERATOR_KIND=SET_YOUR_OPERATOR_KIND

```
#### Create Operator SDK project and implement required operational logic
```bash
mkdir -p  $GOPATH/src/github.com
cd $GOPATH/src/github.com
operator-sdk new $OPERATOR_NAME --vendor=true
cd $OPERATOR_NAME
```

```bash
operator-sdk add api --api-version=${GROUP_NAME}.okto.io/v1alpha1 --kind=${OPERATOR_KIND}
```

Define scope and status, by editing the `pkg/apis/${GROUP_NAME}/v1alpha1/${OPERATOR_KIND}_types.go` file

Update generated code by running following command
```bash
operator-sdk generate k8s
```

Update OpenAPI by running the following command
```bash
operator-sdk generate openapi
```

Sync all deps by running 
```bash
go mod vendor
```

Add new controller 
```bash
operator-sdk add controller --api-version=${GROUP_NAME}.okto.io/v1alpha1 --kind=${OPERATOR_KIND}
```

Register 3rd Party Resources (the OCP Router) in the `cmd/manager/main.go` 
Update `add` function in `pkg/controller/${OPERATOR_KIND}/${OPERATOR_KIND}_controller.go` and add  
* Watch for Service
* Watch for ConfigMap
* Watch for Route

Update the `Reconsile` loop and add functionality for managing 
* Deployment
* Service
* ConfigMap
* Route

### Test your operator 
