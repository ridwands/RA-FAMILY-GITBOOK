# Preparation

1. ​[Github Repo (INFRA-MANIFEST)](https://github.com/ridwands/INFRA-MANIFEST)​

folder github has approximately like below.&#x20;

* apps ->  apps folders we have
* platform -> tool folders

```
.
├── apps
│   ├── ORDER-SERVICE
│   ├── MEMBER-SERVICE
└── platform
    └── argocd
        ├── base
        └── overlays
            └── staging
```

2. [Kustomization](https://kubectl.docs.kubernetes.io/installation/kustomize/)
3. [SOPS](https://github.com/getsops/sops)
4. ​[Minikube](https://minikube.sigs.k8s.io/docs/start/?arch=%2Fmacos%2Farm64%2Fstable%2Fbinary+download)
5. Minikube Ingress

install ingress nginx with type command

```

minikube addons enable ingress
```
