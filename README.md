
Deploy sample microservices into EKS to test cluster.

## Quick Start

#### Deploy

- We can watch the deploying progress by looking at the status in another terminal:

    ```bash
    watch kubectl get all -o wide
    ```


- Let’s bring up the `NodeJS Backend API`:

    ```bash
    cd nodejs
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    ```

-   Bring up the `Crystal Backend API`:

    ```bash
    cd crystal
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    ```

- Bring up the `Ruby Frontend`:

    ```bash
    cd frontend
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
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
cd frontend
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml

cd crystal
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml

cd nodejs
kubectl delete -f service.yaml
kubectl delete -f deployment.yaml

```