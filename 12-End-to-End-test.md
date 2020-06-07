# CKA 12: End-to-End test

* kubetest: github test-infra
* e2e: 〜1000 : 12 hours
* conformance: 〜160 : 1.5 hours
* sonobuoy

## kubetest

    go get -u k8s.io/test-infra/kubetest
    kubetest --extract=v1.xx.x
    cd kubernetes
    kubetest --test --provider=skeleton

https://github.com/mmumshad/kubernetes-the-hard-way/blob/master/docs/16-e2e-tests.md

## Smoke test

https://github.com/mmumshad/kubernetes-the-hard-way/blob/master/docs/15-smoke-test.md
