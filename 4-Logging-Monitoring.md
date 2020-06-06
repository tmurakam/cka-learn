# CKA 4: Logging / Monitoring

## モニタリング

- metrics-server
- prometheus
- ...

kubelet 上で cAdvisor という agent が動作し、メトリクスを収取。server に送る。

kubectl top node
kubectl top pod

## Application log

kubectl logs -f <pod> [<container_name>]

マルチコンテナ Pod の場合はコンテナ名が必要なので注意。
