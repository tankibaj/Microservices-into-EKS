
Deploy sample microservices into EKS to test cluster.

## Quick Start

#### Deploy

- We can watch the deploying progress by looking at the status in another terminal:

    ```bash
    watch kubectl get all -o wide
    ```


- Let’s bring up the `NodeJS Backend API`:

    ```bash
    kubectl apply -f nodejs/deployment.yaml
    kubectl apply -f nodejs/service.yaml
    ```

-   Bring up the `Crystal Backend API`:

    ```bash
    kubectl apply -f crystal/deployment.yaml
    kubectl apply -f crystal/service.yaml
    ```

- Bring up the `Ruby Frontend`:

    ```bash
    kubectl apply -f frontend/deployment.yaml
    kubectl apply -f frontend/service.yaml
    ```

- Find the `ELB` url and `curl` it:

    ```bash
    ELB=$(kubectl get service ecsdemo-frontend -o json | jq -r '.status.loadBalancer.ingress[].hostname')
    
    echo "http://$ELB"
    curl -m3 -v $ELB
    ```

- Scale up the backend services:

    ```bash
    kubectl scale deployment ecsdemo-nodejs --replicas=3
    kubectl scale deployment ecsdemo-crystal --replicas=3
    ```

- Scale up the frontend service:

    ```bash
    kubectl scale deployment ecsdemo-frontend --replicas=3
    ```
---

#### Clean up

To delete the resources created by the applications, we should delete the application deployments:

```bash
kubectl delete -f frontend/service.yaml
kubectl delete -f frontend/deployment.yaml

kubectl delete -f crystal/service.yaml
kubectl delete -f crystal/deployment.yaml

kubectl delete -f nodejs/service.yaml
kubectl delete -f nodejs/deployment.yaml
```