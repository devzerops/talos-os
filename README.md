# talos-os# talos-os


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

# n100 intel bios issue

## intel iso

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
