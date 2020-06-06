# CKA 5: Application LifeCycle Management

## Rolling Update / Rollback

kubectl rollout status deployment <name>

Strategy:
- recreate: 全部捨てて作り直す
- rolling update: 1個ずつ

ロールバック

kubectl rollout undo deployment <name>

イメージだけ差し替え

kubectl set image deployment <name> nginx=nginx:1.9.1

## Command

Docker
* CMD, ENTRYPOINT (試験に出るわけではないが
* ENTRYPOINT でプログラム指定し CMD で引数指定する感じ
 
Pod:
* command: [] => Docker ENTRYPOINT
* args: [] => Docker CMD 相当

command と CMD は対応しないので注意

## Environment Variables

Pod に環境変数を投入する方法。

env: 環境変数名を Pod に指定する方法
* name: value:
* valueFrom:
    * configMapKeyRef:  ConfigMap の特定のキー
    * valueFrom.secretKeyRef:

envFrom: 環境変数名を ConfigMap から持ってくる方法
* configMapRef: ConfigMap 全部
* secretRef:

## ConfigMaps

kubectl create configmap <name> —from-leteral=<key>=<value>

複数指定したい場合は —from-leteral を複数回書く。
—from-file=… で property ファイルからロードもできる。

Pod の env: で読み込み、volumes.configMap で読み込み、など。

volume の場合は、key 毎にファイルができる。

## Secrets

 kubectl create secret generic <name> —from-literal=<key>=<value>

generic を指定する点に注意。他に tls, docker-registry type がある。
作成時に base64エンコードは不要。

yamlで作る場合(宣言的)、value は base64 エンコードしないといけない。
実施するときは echo -n 'xxx' | base64 
デコード次は base64 --decode

## Multi Container Pod

特になし

## Init Containers

特になし

# Liveness Probe / Readiness Probe

これは CKA 範囲外
