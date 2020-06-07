# CKA 9: Networking

## Pre-requisite

### Routing

    ip link
    ip addr
    ip addr add 192.168.1.10/24 dev eth0
    ip route
    ip route add 192.168.1.0/24 via 192.168.2.1
    cat /proc/sys/net/ipv4/ip_forward
    arp
    netstat -plnt

### DNS

### CoreDNS

### Network Namespaces

Netowk Namespace 毎に仮想インタフェース(veth) と ARP, Routing table がある。

    # create namespace 
    ip netns add <name>

    # exec in network ns
    # ip netns exec <name> <command> または ip -n <name> <command> 
    ip -n <name> link
    ip -n <name> arp
    ip -n <name> route

pipe (仮想ケーブル)を作り、ns 間を接続する

    ip link add veth-red type veth peer name veth-blue
    ip link set veth-red netns red
    ip link set veth-blue netns blue
    
仮想ブリッジを作成してここに接続する。

...

※ Kubernetes では Pod 毎に1個の Network Namespace を割り当てる。

### Docker networking

* Host network: ホストネットワークを共有
* Bridge network: 仮想ブリッジ接続
    * コンテナ毎に network namespace を作る

## CNI

標準的な Interface.
Namespace 作成、コンテナの ADD/DEL、IPアドレス割当、など。

Docker 自身は CNI をサポートしない。
k8s は Docker を none ネットワークで作成し、CNI で接続する。

## Cluster Network

* kube-apiserver: 6443
* kubelet: 10250
* scheduler: 10251
* controller-manager: 10252
* etcd: 2379, 2380(peer)
* services: 30000-32767

## Pod Networking / CNI in Kkubernetes

Pod は固有 IP を持ち、互いに NAT なしに通信できる。

コンテナネットワーク設定は CNI が行う。
CNI は kubelet が呼び出す。 --cni-bin-dir, --cni-conf-dir などで指定。

/opt/cni/bin, /etc/cni/net.d

## Weaveworks

Node に Agent. IP カプセリング. DaemonSets.

## IPAM: IP Address Management (CNI)

IPアドレス管理は CNI が行う。

## Service Networking

kube-proxy が service の forwarding rule を生成する。

userspace, iptables, ipvs モードがある。

service の IP アドレス範囲は、kube-apiserver の --service-cluster-ip-range で指定。

## Cluster DNS





