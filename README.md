# Deploy sample microservices into EKS

![Imgur](https://i.imgur.com/eyDfhcw.png)


## Quick Start

#### Deploy

- We can watch the deploying progress by looking at the status in another terminal:

    ```bash
    watch kubectl get all -o wide
    ```


- Let’s bring up the `NodeJS Backend API`:

    ```bash
    kubectl apply -f https://git.io/nodejs-deployment
    kubectl apply -f https://git.io/nodejs-service
    ```

-   Bring up the `Crystal Backend API`:

    ```bash
    kubectl apply -f https://git.io/crystal-deployment
    kubectl apply -f https://git.io/crystal-service
    ```

- Bring up the `Ruby Frontend`:

    ```bash
    kubectl apply -f https://git.io/frontend-deployment
    kubectl apply -f https://git.io/frontend-service
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
kubectl delete -f https://git.io/frontend-deployment
kubectl delete -f https://git.io/frontend-service

kubectl delete -f https://git.io/crystal-deployment
kubectl delete -f https://git.io/crystal-service

kubectl delete -f https://git.io/nodejs-deployment
kubectl delete -f https://git.io/nodejs-service
```