# CKA 10: Kubernetes the hard way

## Design a cluster

## HA Cluster

* apiserver : Active-Active
* scheduler, controller-manager: Active-Standby
    * leader election: 15秒間隔でリース期間ロック。10秒毎にリース更新。2秒毎にリーダ獲得を試みる。

## HA etcd

* RAFT
* Leader - Follower
* Write は過半数(quorum)書き込みが完了するまで

# Kubernetes-the-hard-way Vagrant version

https://github.com/mmumshad/kubernetes-the-hard-way

# TLS Bootstrap

kubelet のクライアント証明書・サーバ証明書の生成

1. bootstrap token を作成し system:bootstrappers に紐付ける
2. system:node-bootstrapper role を system:bootstrappers group に assign
    * これは CSR を submit する role
3. system:certificates.k8s.io:certificatesiningrequests:nodeclient を system:bootstrappers に assign 
    * これは CSR を auto approve する role
4. system:certificates.k8s.io:certificatesiningrequests:selfnodeclient を system:nodes に assign
    * これは CSR を auto renew する role 
    
kubelet はこの token を使って CSR を apiserver に登録し Approve する。

kubelet クライアント証明書は auto approve できるが、サーバ証明書は手動で approve が必要。

    
