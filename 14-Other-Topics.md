# CKA 14: Other topics

## kubectl with JSON Path

    kubectl get pods -o=jsonpath=.items[0].spec.containers[0].image
