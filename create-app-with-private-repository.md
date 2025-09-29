# Create App With  Private Repository

Goal :&#x20;

* Be able to create app with private repository

Step :&#x20;

* do patch in platform/argocd/overlays/staging
* create repo-secret.env

```
url=https://github.com/ridwands/REPOSITORY.git
username=git
password=$(OUR TOKEN)
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

* create apps

structure of repository is both between platform and apps

```
├── apps
│   └── nginx
│       ├── deployment.yaml
│       ├── ingress.yaml
│       ├── kustomization.yaml
│       └── service.yaml
└── platform
    └── argocd
        ├── base
        │   ├── kustomization.yaml
        │   └── namespace.yaml
        └── overlays
            └── staging
                ├── argocd-cm-patch.yaml
                ├── argocd-cmd-params-patch.yaml
                ├── argocd-ingress.yaml
                ├── argocd-repo-server-patch.yaml
                ├── encrypt-repo-secret.yaml
                ├── kustomization.yaml
                └── repo-secret.env
```

* create app nginx

1. apps/nginx/deployment.yaml

```
```
