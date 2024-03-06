---
comments: true
---

# Namespaces and Keptn

This page discusses how to restrict the namespaces
where Keptn and the
[lifecycle-operator](../../components/lifecycle-operator/index.md)
run.

For information about how to allocate Keptn resources
across namespaces, see
[Namespaces and resources](namespace-resources.md).

## Default behavior

Keptn must be installed on its own namespace
that does not run any other components,
especially any application deployment.

By default, Keptn lifecycle orchestration is enabled
for all namespaces except the followings:

- `kube-system`
- `kube-public`
- `kube-node-lease`
- `cert-manager`
- `observability`
- `monitoring`
- `<Keptn installation namespace>`

## Custom namespace restriction

If you want to restrict Keptn to only some namespaces, you should:

- Allow those namespaces during installation
- Annotate those namespaces

To implement this:

1. Create a `values.yaml` file
   that lists the namespaces Keptn lifecycle orchestration should monitor:

      ```yaml
      lifecycleOperator:
        allowedNamespaces:
        - allowed-ns-1
        - allowed-ns-2
      ```

1. Add the values file to the helm installation command:

      ```shell
      helm repo add keptn https://charts.lifecycle.keptn.sh
      helm repo update
      helm upgrade --install keptn keptn/keptn -n keptn-system \
           --values values.yaml --create-namespace --wait
      ```

1. Annotate the namespaces where Keptn lifecycle orchestration is allowed
   by issuing the following command
   for each namespace:

      ```shell
      kubectl annotate ns <your-allowed-namespace> \
            keptn.sh/lifecycle-toolkit='enabled'
      ```

> **Note**
When this option is set, you must update the Helm installation
by adding the names of additional namespaces you create
to your `values.yaml` file
and rerunning the `helm upgrade` command.