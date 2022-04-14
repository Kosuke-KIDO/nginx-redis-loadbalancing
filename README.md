# nginx-redis-loadbalancing

## Installation

### RUN k8s

If you do not installed yet,
install [kubectl](https://kubernetes.io/docs/tasks/tools/#kubectl),
[kustomize](https://kubectl.docs.kubernetes.io/installation/kustomize/binaries/),
[kind](https://kind.sigs.k8s.io/docs/user/quick-start/#installation)

- My Environment

    ```sh
    kubectl version
    # Client Version: version.Info{Major:"1", Minor:"22", GitVersion:"v1.22.6", GitCommit:"f59f5c2fda36e4036b49ec027e556a15456108f0", GitTreeState:"clean", BuildDate:"2022-01-19T17:33:06Z", GoVersion:"go1.16.12", Compiler:"gc", Platform:"linux/amd64"}

    kustomize version
    # {Version:kustomize/v4.5.4 GitCommit:cf3a452ddd6f83945d39d582243b8592ec627ae3 BuildDate:2022-03-28T23:12:45Z GoOs:linux GoArch:amd64}
    
    kind version
    # kind v0.12.0 go1.17.8 linux/amd64
    ```
- Create a local cluster

    ```sh
    kind create cluster --config kind.yaml
    ```

- Apply manifest

    ```sh
    kustomize build k8s/overlays/local/ | kubectl apply -f -
    ```

- Test

    ```sh
    for i in {0..10}; \
    do \
        redis-cli -p 36379 -c info server | grep run_id; \
    done;
    # -> run_id should be about 8:2
    ```
