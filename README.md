To bootstrap OpenShift GitOps on the cluster, run the following `oc apply` command:

```sh
oc apply -f bootstrap/gitops.yaml
```

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
  url: https://github.com/<username>/aro-rhdh
  username: <username>
  password: <password>
```

Then `oc apply` the secret:

```sh
oc apply -f secrets/gitops-repository-secret.yaml
```
