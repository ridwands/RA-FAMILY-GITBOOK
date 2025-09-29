# Create App With  Private Repository

* create repo-secret.env

```
url=https://github.com/ridwands/INFRA-MANIFEST.git
username=git
password=$(OUR TOKE)
project=default
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



