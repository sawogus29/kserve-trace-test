## Install KServe
- [kserve quickstart](https://kserve.github.io/website/latest/get_started/#install-the-kserve-quickstart-environment)
- Skip `kind` install

```bash
curl -s "https://raw.githubusercontent.com/kserve/kserve/release-0.13/hack/quick_install.sh" | bash
# if above installation fails, then try again
helm install kserve-crd oci://ghcr.io/kserve/charts/kserve-crd --version v0.13.0
helm install kserve oci://ghcr.io/kserve/charts/kserve --version v0.13.0
```

## Test InferenceService
- [First Inference](https://kserve.github.io/website/latest/get_started/first_isvc/)

```bash
kubectl create namespace kserve-test
kubectl -n kserve-test apply -f sklearn-iris.yaml
```

### (Optional) Swagger UI

```bash
kubectl edit svc istio-ingressgateway -n istio-system
# LoadBalancer -> NodePort
```
- Check 80:PORT
- Edit hosts file 
- http://sklearn-iris.kserve-test.example.com:{PORT}/docs#

## (Knative) Trace Config
```bash
kubectl patch --namespace knative-serving configmap/config-tracing \
--type merge \
--patch '{"data":{"backend":"zipkin","zipkin-endpoint":"http://jaeger.observability:9411/api/v2/spans", "debug": "true"}}'
```


## Debug `kserve-container`
```bash
# Attach Debug Container
kubectl debug -it pod/sklearn-iris-predictor-00002-deployment-7cdb7955f8-s244d --image=nicolaka/netshoot --target kserve-container -- /bin/bash

# (Inside Debug Container) Watch HTTP
ngrep -W byline -d any port 8080

# (Outside) Send HTTP Request
curl -v -H "Content-Type: application/json" http://sklearn-iris.kserve-test.example.com:31976/v1/models/sklearn-iris:predict -d @./iris-input.json
```


 
