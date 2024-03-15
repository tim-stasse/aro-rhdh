To bootstrap OpenShift GitOps on the cluster, run the following commands:

```sh
export gitops_repo=https://github.com/tim-stasse/aro-rhdh.git
export cluster_base_domain=$(oc get ingress.config.openshift.io cluster --template={{.spec.domain}} | sed -e "s/^apps.//")
oc apply -k bootstrap/openshift-gitops-operator
envsubst < bootstrap/argocd.yaml | oc apply -f -
```

<details>
<summary>(optional) create a secret for private gitops repo</summary>

Create a file named `gitops-repository-secret.yaml` inside the `secrets` folder

```sh
mkdir secrets
touch secrets/gitops-repository-secret.yaml
```

Use an example secret from: https://argo-cd.readthedocs.io/en/stable/operator-manual/argocd-repositories-yaml/

Paste the contents into `gitops-repository-secret.yaml` and update it with real values, for example:

```yaml
apiVersion: v1
kind: Secret
metadata:
  name: gitops-repository
  namespace: openshift-gitops
  labels:
    argocd.argoproj.io/secret-type: repository
stringData:
  url: https://github.com/tim-stasse/aro-rhdh
  username: tim-stasse
  password: <password>
```

Then `oc apply` the secret:

```sh
oc apply -f secrets/gitops-repository-secret.yaml
```
---
</details>

Then `oc apply` the root app:

```sh
envsubst < bootstrap/root-applications.yaml | oc apply -f -
```

Here is me making some changes for a Pull Request (PR)
