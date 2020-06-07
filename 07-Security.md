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

手順は https://kubernetes.io/docs/tasks/tls/managing-tls-in-a-cluster/

### ユーザ証明書

* k8s クラスタはユーザを管理していない
* 証明書の CN にユーザ名を書くだけ
* O に system:masters を入れておくと cluster-admin 権限が得られる (RoleBinding 不要)
* ユーザは namespace には紐付かない(あたりまえだが)

https://qiita.com/knqyf263/items/aefb0ff139cfb6519e27 を参照。

### CertificateSigningRequest object 作成時の注意事項

* CSR base64 改行は削る。cat xxx.csr | base64 | tr -d '\n' しておく。
* group: を書く
    * **system:authenticated** を指定する
    * kubernetes.io の Manage TLS ... のページにはこれが入ってないので注意。

## kubeconfig

--kubeconfig で指定。デフォルトは ~/.kube/config

clusters / contexts / users

    kubectl config view
    kubectl config use-context <context>

## API Groups

core group と named group に分かれる。

apiVersion の先頭がいきなり /v1 とかのものが core group。
そうでないもの (apps/v1 とか) は named group

* core group (/api 配下)
    * namespace, pod, pv, configmap, service など
* named group (/apis 配下)
    * /apps: /v1/deployments, replicasets, statefulsets
    * /networking.k8s.io: networkpolicies
    * /certificates.k8s.io: 証明書関連
    * ...
    
Tips: kubectl proxy コマンドを実行すると、api-server への proxy ができる。認証なしで curl できる。

## RBAC

* Role
    * rules: apiGroup, resources, verbs でルール指定
        * apiGroup の "" は core group を指定
        * 個別リソースを指定したい場合は resourceNames で
* RoleBinding

Role 作成

    kubectl create role  <name> --resource <...> --verb=<...>
    
RoleBinding 作成

    kubectl create rolebinding <name> --role=<role> --user=<user>    

アクセス可能チェック

    kubectl auth can-i [action]
    kubectl auth can-i [action] --as [username]

Note:

* Deployment 権限を与える場合、apiGroup には "apps" だけでなく "extensions" も書く必要がある。

## Cluster RBAC

Cluster wide な role / rolebinding

## Image Security

## Security Context

## Network Policy
    