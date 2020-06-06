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

CSR署名

    openssl x509 -req -in xxx.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out out.crt -days 365

## Certificate workflow

Certificates API

* CertificateSigningRequest Object を作成
    * https://kubernetes.io/docs/reference/access-authn-authz/certificate-signing-requests/
* Review
    * kubectl get csr
* Approve
    * kubectl certificate approve <name>
* Get
    * kubectl get csr <name> -o yaml
    * status.certificates: に base64 エンコードされた証明書が入っている

これらの処理は、CSR-APPROVING, CSR-SIGNING Controller などが実行する。
CA証明書・鍵は controller-manager の起動時オプションに指定している (--cluster-signing-cert-file など)
