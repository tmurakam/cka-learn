# CKA 13: Troubleshooting

## Application failure

* Check service status
    * kubectl describe
* Check pod
    * kubectl logs
* Check dependent services    

## Control plane failure

    kubectl get nodes
    kubectl get pods
    kubectl get pods -n kube-system
    service kubelet status
    service kube-proxy status
    kubectl logs ...
    journalctl -u kubelet

## Worker node failure

    kubectl get nodes
    kubectl describe node ...
    top
    df -h
    service kubelet status
    journalctl -u kubelet
    openssl x509 -in /var/lib/kubelet/worker-1.crt -text

## Network failure

