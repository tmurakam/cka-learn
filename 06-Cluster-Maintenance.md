# CKA 6: Cluster Maintenance

## OS Upgrade

* Node 停止5分で、Pod は死んだとみなされる (その前に復帰すれば戻る)
    * これは eviction timeout。controller-manager の引数で 5m0sに設定
    * 5分に戻るのであればそのまま再起動してもいいが、drain するのが望ましい
* drain / uncordon / cordon
* drain すると ReplicaSet でない Pod は削除される (--force が必要)

## Cluster Upgrade

* api-server バージョン X
* controller-manager, scheduler は X-1 まで
* kubelet, kube-proxy は X-2 まで
* kubectl は X-1 から X + 1 まで
* サポートは最新3バージョンまで

* アップグレードするときは:
    * kubeadm upgrade plan / upgrade apply
* 最初に master 次に worker
    * master コンポーネント -> kubelet の順
* worker は1台ずつ。drain - upgrade - uncordon.
* または新規ノードを追加し、古いノードを削除していく

## Backup & Restore

* Resource config
    * yaml 全部取っとくのが原則。git などに。
    * api server アクセスして yaml バックアップできる
        * kubectl get all --all-namespaces -o yaml > all.yaml
        * これで全部取れるわけではない。ARK, Heptio, Velero などの solution 使う。
* etcd cluster
    * etcdバックアップ取るという手段。
    * etcd の --data-dir= にデータディレクトリがある。
    * または snapshot 取る。ETCD_API=3 etcdctl snapshot save xxx.db
    * リストア。api server 止めて、etcdctl snapshot restore で戻す。cluster-token 新しく。

詳細は https://github.com/mmumshad/kubernetes-the-hard-way/blob/master/practice-questions-answers/cluster-maintenance/backup-etcd/etcd-backup-and-restore.md
を参照。これは大変。。。

ECTDCTL_API=3 ...