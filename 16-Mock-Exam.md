# CKA 16: Mock Exam

## Exam 1

* 比較的容易なものだけ
* JSON Path くらいか

## Exam 2

* etcd バックアップ
* emptyDir 付きで指定 Pod 作成
* system_time 変更可能な Pod 作成
* PVC作成して Pod mount
* Deployment 作成。Rolling update 実行。resource annotation に update を record する。
    * コマンドラインでバージョン指定が必要。kubectl edit / kubectl set image を使う必要がある。
* User 作成。特定 namespace に対して指定した operation の権限を与える。CSR/Key を使う。
* Pod 作成. Service expose.
* Static pod 作成. Restart されること。

## Exam 3

* Service Account 作成, PVC アクセス権 Role, RoleBinding 作成. Pod 作成し Service Account 設定。
* Internal IP 取得. jsonpath 使用
* Multi container pod 作成
* Pod 作成。runAsUser, fsGroup 指定。
* Network不通調査。NetworkPolicy作成
* Node taint. Pod toleration 指定した taint ノードに deploy.
* Pod 作成。label
* kubeconfig を修正せよ
* Deployment が起動しない理由を調査せよ (control plane 異常)
