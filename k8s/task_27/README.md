 kubectl create secret generic grafana-admin-credentials --from-file=./admin-user --from-file=admin-password -n monitoring

 kubectl describe secret -n monitoring grafana-admin-credentials
 nano values.yaml
 helm install -n monitoring prometheus prometheus-community/kube-prometheus-stack -f values.yaml
 kubectl port-forward -n monitoring grafana-fcc55c57f-fhjfr 52222:3000

#If you make changes to your values.yaml you can deploy these changes by running
helm upgrade -n monitoring prometheus prometheus-community/kube-prometheus-stack -f values.yaml