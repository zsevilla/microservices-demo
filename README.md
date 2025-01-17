# Prerequisites

## 1. Create a new 2 nodes EKS cluster (20m)

-   Active AWS Cloud account and the following CLI tools installed
    -   [aws-cli](https://docs.aws.amazon.com/cli/latest/userguide/getting-started-install.html)
    -   [eksctl](https://docs.aws.amazon.com/eks/latest/userguide/eksctl.html)
    -   [kubectl](https://kubernetes.io/docs/tasks/tools/)

```bash
# login to AWS
aws configure

# create new Managed nodes cluster (not the Fargate), this will create 2 nodes cluster (m5.larger)
eksctl create cluster --name pixiecluster --region us-east-1

# download correct kubeconfig
eksctl utils write-kubeconfig --cluster=pixiecluster --region us-east-1

# confirm can connect to the cluster
kubectl get nodes -o wide

# install helm CLI (https://helm.sh/docs/intro/install/)
brew install helm
```

## 2. Sign up for Free New Relic One account (5m)

-   Sign up for New Relic One account for free, no Credit Card required [here](https://newrelic.com/signup) and get your Ingest API key by:
    -   Open https://one.newrelic.com/api-keys
    -   Click on `Create a key`, select `Ingest - License` and give it a name
    -   Click on 3 dots and select `Copy key`, you will need this during your hands on workshop

## 3. Deploy Wavesocks application to your cluster (10m)

-   Deploy this [e-commerce microservice application](https://github.com/nvhoanganh/microservices-demo/blob/master/internal-docs/design.md) to the cluster

```bash
# create new namespace
kubectl create namespace sock-shop && \
kubectl apply -f "https://raw.githubusercontent.com/nvhoanganh/microservices-demo/master/deploy/kubernetes/complete-demo.yaml" \
  --namespace=sock-shop

# make sure all containers are running properly
kubectl wait --for=condition=available --timeout=450s --all deployments -n sock-shop

# get the external IP of the front-end service
kubectl get service --watch --namespace=sock-shop

```

-   open `http://<EXTERNAL-IP>` and make sure you can see the application
    ![](screenshots/homepage.png)