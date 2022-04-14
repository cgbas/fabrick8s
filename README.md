# fabrick8s

Meus estudos baseados em A hitchhikers guide to deploying Hyperledger Fabric on kubernetes 

Talk no TDC: https://s3-sa-east-1.amazonaws.com/thedevconf/presentations/TDC2018POA/blockchain/DKH-6936_2018-12-12T062113_hfl_kubernetes_TDC.pdf[^1]



***
## O Ambiente
### Cluster local

Vamos começar com um cluster local com 4 nós[^1] e a maneira mais prática para conseguir é com o [Kind](https://kind.sigs.k8s.io/). 
Para isso, execute[^2]

```bash

go install sigs.k8s.io/kind@v0.12.0

 kind create cluster  --name fabrick8s
```

#### Cert-manager

Agora, instale o cert manager no cluster

```bash
helm install stable/cert-manager -n cert-manager --namespace cert-manager
```

E configure os cert-managers de Staging e Produção

```bash
kubectl create -f ./lf-k8s-hlf-webinar/extra/certManagerCI_staging.yaml

kubectl create -f ./lf-k8s-hlf-webinar/extra/certManagerCI_production.yaml
```

## Fabric CA


```
helm install stable/hlf-ca -n ca --namespace blockchain -f ./helm_values/ca_values.yaml
CA_POD=$(kubectl get pods -n blockchain -l "app=hlf-ca,release=ca" -o jsonpath="{.items[0].metadata.name}")
kubectl logs -n blockchain $CA_POD | grep "Listening on"
```


---
[^1]: O [repo](https://github.com/aidtechnology/lf-k8s-hlf-webinar) que originou essa talk está adicionado como um submódulo .
[^2]: A configuração do cluster está no arquivo `kind.yaml`

