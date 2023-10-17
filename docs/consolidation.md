# Karpenter Consolidation Demo


## Commands

```
export CLUSTER_NAME=eks-karpenter
cd provisioner
envsubst < basic.yaml | kubectl apply -f -
eks-node-viewer --node-selector "karpenter.sh/provisioner-name" --resources cpu
kubectl apply -f ../workload/basic-deploy.yaml
kubectl scale deployment inflate --replicas 5
kubectl rollout status deployment inflate --timeout=180s
kubectl -n karpenter logs deployments/karpenter -c controller
envsubst < basic-with-consolidation.yaml | kubectl apply -f -
kubectl get node -l type=karpenter
```


## Set Up Basic Provisioner

## Deploy Workload

## Enable Consolidation

## Enable Spot