# talos-os

# n100 intel bios issue
If you boot from a USB, you must add the above plugin to boot.
When updating the control-plane or worker using talosctl, you must add the above plugin, otherwise it will result in an infinite boot loop.

## intel iso

![intel-n100-bios issue](https://github.com/siderolabs/talos/issues/9369)

``` bash
wget https://factory.talos.dev/image/a67adf43d3bcbfb16d4fb6227edcf6f5ff039c493372f759d2dfcf8c21c932df/v1.8.3/metal-amd64.iso
```

## intel path file

``` yaml
customization:
    systemExtensions:
        officialExtensions:
            - siderolabs/i915-ucode
            - siderolabs/mei
```


``` bash
factory.talos.dev/installer/a67adf43d3bcbfb16d4fb6227edcf6f5ff039c493372f759d2dfcf8c21c932df:v1.8.3
```



# start

``` bash
talosctl gen config --config-patch @patch.yaml --config-patch @intel-path.yaml my-cluster https://192.168.0.2:6443 --output _out
```

``` bash
talosctl apply-config -f _out/control-plane.yaml -n 192.168.0.2 -e 192.168.0.2 --insecure
```

``` bash
talosctl config endpoints 192.168.0.2
talosctl config nodes 192.168.0.2
```

``` bash
talosctl bootstrap
```


``` bash
talosctl apply-config -f _out/worker.yaml -n 192.168.0.4,192.168.0.5,192.168.0.6
```


``` bash
root@homelab:/home/server/root/configs/_out# talosctl get members
NODE          NAMESPACE   TYPE     ID              VERSION   HOSTNAME        MACHINE TYPE   OS               ADDRESSES
192.168.0.2   cluster     Member   talos-am5-o43   1         talos-am5-o43   worker         Talos (v1.8.3)   ["192.168.0.5"]
192.168.0.2   cluster     Member   talos-f8a-itv   4         talos-f8a-itv   controlplane   Talos (v1.8.2)   ["192.168.0.2"]
192.168.0.2   cluster     Member   talos-l8o-sk4   1         talos-l8o-sk4   worker         Talos (v1.8.3)   ["192.168.0.6"]
192.168.0.2   cluster     Member   talos-ujg-qeb   1         talos-ujg-qeb   controlplane   Talos (v1.8.2)   ["192.168.0.3"]

```


# cilium install

``` bash
talosctl kubeconfig
```

``` bash
helm upgrade --install cilium cilium/cilium -f values.yaml -n kube-system
```

``` bash
root@homelab:/home/server/root/k8s/cilium# kubectl top nodes
NAME            CPU(cores)   CPU%   MEMORY(bytes)   MEMORY%   
talos-am5-o43   438m         2%     3136Mi          11%       
talos-f8a-itv   397m         10%    4415Mi          29%       
talos-l8o-sk4   473m         2%     4284Mi          15%       
talos-ujg-qeb   434m         10%    6426Mi          42%    
```
