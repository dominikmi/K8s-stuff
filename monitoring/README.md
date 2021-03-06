# Prometheus & Grafana setup

1) Create manually Storage Class definition: `manual-storage-class.yaml`
2) Create two PV: storage-volume & config-volume in Longhorn -> create Volumes, name them, create PV (not PVC!) and do not attach them. Prometheus will take care of it.
3) Install Prometheus `helm install prometheus stable/prometheus --set server.storageClass=manual`

After a moment (in around 90 seconds) you should see in your longhorn panel something like this:

![longhorn PVs](files/pic-m1.png)

4) In `monitoring` forlder mkdir `grafana`
6) cd `grafana` and create two files:
	- grafana-configmap.yaml
	- grafana-values.yaml
7) apply the configmap for Grafana: `kubectl apply -f grafana-configmap.yaml`
8) Install Grafana `helm install grafana stable/grafana -f grafana-values.yaml --namespace monitoring`
9) Extract default password for grafana admin user: `kubectl get secret --namespace monitoring grafana \ -o jsonpath="{.data.admin-password}" | base64 --decode ; echo`
9) Get to http://<your_grafana_ui_IP>:3000 and log in with `admin` and `password`
