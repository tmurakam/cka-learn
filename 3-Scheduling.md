3: Scheduling

# selector

Service の場合は単に selector に条件
Deployment など新しい object の場合は label selector を使う。matchLabels など。

# selector
kubectl get …. —selector app=Hoge

# taints / tolerations

Taints は Node に、Tolerations は Pod につける
Key-Value pair である。

$ kubectl taint nodes node-name key=value:<effect>

Effect は NoSchedule, PrefereNoSchedule, NoExecute

Tolerations は pod に指定. Key, operator, effect を書く

# Node selector

ノードにラベルをつける。selector でも affinity でもこの作業はまず必要。

$ kubectl label nodes <node-name> <key>=<value>

Pod の nodeSelector でノードを選択できる。

# Node Affinity

matchExpressions でノード選択ができるので強力。だけど文法がめんどい。
pod.spec.affinity.nodeAffinity.requiredDuringSchedulingIgnoredDuringExecution.nodeSelectorTerms.matchExpressions
なげえよ。。。

operator: In, NotIn, Exists, DoesNotExist, Gt, Lt

# Taints と Affinity

両方使う。Taints で意図しない Pod がスケジュールされるのを避けておいて、その上で affinity で指定ノードに入れる。。。

# Resource requests / limits

requests / limits

Note: デフォルト値は LimitRange で指定する。

# DaemonSets

Tips: Deployments yaml 作って Kind を DaemonSet に変えればよい

# Static Pods

Pod の command は配列なので注意。数字はクォート必要。
kubectl で作るときは、—command — … で。

kubectl run --restart=Never --image=busybox static-busybox --dry-run -o yaml --command -- sleep 1000

# Multiple Scheduler

kube-scheduler.yaml を複製して、--scheduler-name= オプションで名前つける。
HA 構成のときは  --leader-elect=true オプションが必要。

カスタムスケジューラを使う場合、Pod の schedulerName: で指定する。

kubectl get events でイベント確認できる。

Udemy: はまったところ。
- scheduler の --port, --secure-port をずらさないといけない(既存のとぶつかる)
- --leader-elect=false にしないといけない。1台だけなので。。。
