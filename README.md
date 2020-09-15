# Create monitoring namespace

```
kubectl apply -f namespace.yml
```

# Create configmaps to mount

```
kubectl create configmap -n monitoring prometheus-config --from-file=prometheus.yml
kubectl create configmap -n monitoring grafana-prov-datasources --from-file=grafana/provisioning/datasources
kubectl create configmap -n monitoring grafana-prov-dashboards --from-file=grafana/provisioning/dashboards
kubectl create configmap -n monitoring grafana-config --from-file=grafana/conf/grafana.ini
kubectl create configmap -n monitoring prometheus-alerting-rules --from-file=alerting_rules.yml
kubectl create configmap -n monitoring alertmanager-config --from-file=alertmanager.yml
kubectl create configmap -n monitoring prometheus-recording-rules --from-file=recording_rules.yml
```

# Create pods and services

```
kubectl apply -f prometheus-deployment.yml
kubectl apply -f grafana-deployment.yml
kubectl apply -f alertmanager-deployment.yml
```

# Create Ingresses

```
kubectl apply -f prometheus.ingress
kubectl apply -f grafana.ingress
kubectl apply -f alertmanager.ingress
```

# Port-forward nginx

```
kubectl port-forward $(kubectl get pods --selector "k8s-app=kube-nginx" --output=name -n kube-system) 8000:80 -n kube-system
```

# Endpoints

- http://localhost:8000/grafana
- http://localhost:8000/prometheus/
- http://localhost:8000/alertmanager/