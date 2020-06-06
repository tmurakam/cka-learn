# CKA 7: Security

## Authentication

User / Service Accounts。
Service Account は CKA には含まれない (CKAD の範囲)

認証は api-server が行う

* Basic認証: --basic-auth-file=xx.csv
    * 当然非推奨
* RBAC

## TLS Certificates

* サーバ証明書
    * apiserver, etcdserver, kubelet
* クライアント証明書
    * scheduler, controller-manager, kube-proxy, kubelet-client, apiserver-kubelet-client, apiserver-etcd-client
* CA
    * kubernetes 用、etcd 用の2つ

証明書生成時のメモ:
* admin: Subject の O=system:master を付ける必要がある。これが管理者権限になる。
* api-server: SANs

#