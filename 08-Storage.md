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

* Pod の containers.volumeMounts でマウント
    * name: volume名
    * mountPath: パス
* Pod の volumes で Volume を定義
     * name: volume名 (上と合わせる)
     * hostPath や persistentVolumeClaim を指定

## Persistent Volume

Volume 定義を Pod から切り離す。

* accessModes:
    * ReadOnceMany (ROX)
    * ReadWriteOnce (RWO)
    * ReadWriteMany (RWX)
* capacity:    

## Persistent Volume Claim

* reclaimPolicy: PVCを削除したときにPVをどうするかを決める
    * Delete: 削除する。デフォルト
    * Retain: 削除されないが、二度と使われない(他の PVC からも)
        * 手で削除しないといけない。
    * Recycle: 中身を削除して再利用する (非推奨になった)

## Storage Class / StatefulSet

CKA 対象外。
