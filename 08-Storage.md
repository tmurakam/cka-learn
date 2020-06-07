# CKA 8: Storage

## Docker Storage

* Storage Driver
    * Layered
    * AUFS, ZFS, BTRFS, Device Mapper, Overlay, Overlay2
* Volume Driver Plugin
    * Local, GlusterFS, Ceph, NetApp, EBS, ...
    * docker run ... --volume-driver <name> ...  

volume mount, bind mount

## CSI : Container Storage Interface

RPC。Volume作成・削除、publish、...

## Volume

Pod 内に volume 定義を記述する方法。

Pod.spec.volumes で Volume を定義し、Pod.spec.containers.volumeMount でマウント

## Persistent Volume

Volume 定義を Pod から切り離す。

* accessModes:
    * ReadOnceMany (ROX)
    * ReadWriteOnce (RWO)
    * ReadWriteMany (RWX)
* capacity:    

## Persistent Volume Claim

