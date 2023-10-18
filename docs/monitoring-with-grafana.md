# Setup Grafan Dashboard 

```
export KARPENTER_VERSION=v0.30.0
helm repo add grafana-charts https://grafana.github.io/helm-charts
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts
helm repo update
kubectl create namespace monitoring
kubectl get ns

curl -fsSL https://raw.githubusercontent.com/aws/karpenter/"${KARPENTER_VERSION}"/website/content/en/preview/getting-started/getting-started-with-karpenter/prometheus-values.yaml | tee prometheus-values.yaml
helm install --namespace monitoring prometheus prometheus-community/prometheus --values prometheus-values.yaml

# prometheus UI
export PROM_SERVER_POD_NAME=$(kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=prometheus,app.kubernetes.io/instance=prometheus" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace monitoring port-forward $PROM_SERVER_POD_NAME 9090

curl -fsSL https://raw.githubusercontent.com/aws/karpenter/"${KARPENTER_VERSION}"/website/content/en/preview/getting-started/getting-started-with-karpenter/grafana-values.yaml | tee grafana-values.yaml
helm install --namespace monitoring grafana grafana-charts/grafana --values grafana-values.yaml

# login to grafana UI, admin/pwd
kubectl get secret --namespace monitoring grafana -o jsonpath="{.data.admin-password}" | base64 --decode ; echo
export GRAFANA_POD_NAME=$(kubectl get pods --namespace monitoring -l "app.kubernetes.io/name=grafana,app.kubernetes.io/instance=grafana" -o jsonpath="{.items[0].metadata.name}")
kubectl --namespace monitoring port-forward $G_POD_NAME 3000

kubectl -n monitoring get po

NAME                                                 READY   STATUS    RESTARTS   AGE
grafana-d9ddd85c4-zkhq7                              1/1     Running   0          5h57m
prometheus-alertmanager-0                            0/1     Pending   0          6h4m
prometheus-kube-state-metrics-59f5f44fd-vmz6z        1/1     Running   0          6h4m
prometheus-prometheus-node-exporter-256mq            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-6cpxp            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-8968k            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-95vm9            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-9cx55            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-bf5g7            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-bfq5m            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-ccpzf            1/1     Running   0          6h4m
prometheus-prometheus-node-exporter-dkb4q            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-fgssb            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-fqs4b            1/1     Running   0          5h30m
prometheus-prometheus-node-exporter-h9wrr            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-hfms8            1/1     Running   0          6h4m
prometheus-prometheus-node-exporter-kfrj8            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-kxq7c            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-ltkw9            1/1     Running   0          5h44m
prometheus-prometheus-node-exporter-lwj6p            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-ngmx8            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-p9xnl            1/1     Running   0          6h4m
prometheus-prometheus-node-exporter-pbm5w            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-qsn8x            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-r59zg            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-sgfw7            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-slwst            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-sntr9            1/1     Running   0          5h29m
prometheus-prometheus-node-exporter-thx4d            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-vbg7p            1/1     Running   0          6h4m
prometheus-prometheus-node-exporter-xf6zg            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-xmp89            1/1     Running   0          6h4m
prometheus-prometheus-node-exporter-z8jhr            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-znwx6            1/1     Running   0          5h45m
prometheus-prometheus-node-exporter-zqkr8            1/1     Running   0          5h45m
prometheus-prometheus-pushgateway-75986b9c9f-q82vk   1/1     Running   0          5h44m
prometheus-server-867b9c8754-8l7n7                   2/2     Running   0          6h4m

```

