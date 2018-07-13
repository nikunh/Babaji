#################
#Installation Steps
kubectl apply -f ../storage/prometheus-block-rook-storageclass.yaml
helm del --purge prometheus
helm install stable/prometheus --namespace prometheus --name prometheus -f prometheus-values.yaml

# Grafana helm chart has a lot of issues and is broken. I will work on building my own chart when I have some time.
# Install (do this from the folder where promethes files are)
helm install stable/grafana --namespace prometheus  --name prometheus-grafana -f grafana-values.yaml && kubectl delete -f grafana-pvc.yaml && kubectl apply -f grafana-pvc.yaml && kubectl apply -f prometheus-grafana-svc.yaml 
# Uninstall (do this from the folder where promethes files are)
helm del --purge prometheus-grafana ; kubectl delete -f grafana-pvc.yaml ; kubectl delete -f prometheus-grafana-svc.yaml

Prometheus NOTES:
The Prometheus server can be accessed via port 80 on the following DNS name from within your cluster:
prometheus-server.prometheus.svc.cluster.local


Get the Prometheus server URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace prometheus -l "app=prometheus,component=server" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace prometheus port-forward $POD_NAME 9090


The Prometheus alertmanager can be accessed via port 80 on the following DNS name from within your cluster:
prometheus-alertmanager.prometheus.svc.cluster.local


Get the Alertmanager URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace prometheus -l "app=prometheus,component=alertmanager" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace prometheus port-forward $POD_NAME 9093


The Prometheus PushGateway can be accessed via port 9091 on the following DNS name from within your cluster:
prometheus-pushgateway.prometheus.svc.cluster.local


Get the PushGateway URL by running these commands in the same shell:
  export POD_NAME=$(kubectl get pods --namespace prometheus -l "app=prometheus,component=pushgateway" -o jsonpath="{.items[0].metadata.name}")
  kubectl --namespace prometheus port-forward $POD_NAME 9091

For more information on running Prometheus, visit:
https://prometheus.io/


Grafana NOTES:
1. Get your 'admin' user password by running:

   kubectl get secret --namespace prometheus prometheus-grafana -o jsonpath="{.data.grafana-admin-password}" | base64 --decode ; echo

2. The Grafana server can be accessed via port 80 on the following DNS name from within your cluster:

   prometheus-grafana.prometheus.svc.cluster.local

   Get the Grafana URL to visit by running these commands in the same shell:

     export POD_NAME=$(kubectl get pods --namespace prometheus -l "app=prometheus-grafana-grafana,component=grafana" -o jsonpath="{.items[0].metadata.name}")
     kubectl --namespace prometheus port-forward $POD_NAME 3000

3. Login with the password from step 1 and the username: admin

