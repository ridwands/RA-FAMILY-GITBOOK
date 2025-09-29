# Create App With  Public Repository

* create repo-secret.env

```
```

* create encrypt-repo-secret.yaml

```
apiVersion: goabout.com/v1beta1
kind: SopsSecretGenerator
metadata:
  name: repo-secret
  labels:
    argocd.argoproj.io/secret-type: repository
envs:
  - repo-secret.env
```

* edit kustomization.yaml

```
generators:
  - encrypt-repo-secret.yaml
```



