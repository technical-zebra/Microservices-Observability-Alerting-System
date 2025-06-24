# Microservices Observability & Alerting System

## Prerequistes

- kubectl
- Helm

## Configure Helm Repo

helm repo add grafana https://grafana.github.io/helm-charts
helm repo add prometheus-community https://prometheus-community.github.io/helm-charts

## Install grafana/loki

helm install loki grafana/loki --version 6.29.0 -f ./helm-config/loki-values.yaml -n monitoring

## Installing kube-prometheus-stack

helm install kube-prometheus prometheus-community/kube-prometheus-stack --version 45.7.1 -f ./helm-config/prometheus-values.yaml -n monitoring

# Installing promtail

kubectl apply -n monitoring -f ./helm-config/promtail.yaml

## Implement other components

kubectl apply -f ./deployments/v1.yaml
kubectl apply -f ./PV_PVC/v1.yaml
kubectl apply -f ./services/v1.yaml
kubectl apply -f ./serviceMonitor/v1.yaml
kubectl apply -f ./alarms/prometheus-alert.yaml

## Expose the GUI dashboard

kubectl edit svc kube-prometheus-grafana # change to nodeport
kubectl edit svc kube-prometheus-kube-prome-prometheus # change to nodeport
