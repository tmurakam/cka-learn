# CKA 14: Other topics

## kubectl with JSON Path

[JSONPath Support](https://kubernetes.io/docs/reference/kubectl/jsonpath/) を見ればよい。

-o jsonpath=... で指定する

    kubectl get pods -o jsonpath='{.items[0].spec.containers[0].image}'

loop させる場合の JSONPATH 指定

    {range .items[*]}[{.metadata.name}, {.status.capacity}] {end}

表示するときのカラム追加は -o custom columns で

    kubectl get node -o custom-columns=LABEL:JSONPATH,...
    
ソートする場合は --sort-by で

    kubectl get node --sort-by=.metadata.name
