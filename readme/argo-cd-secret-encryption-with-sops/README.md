# Argo CD Secret Encryption With SOPS

we using [Kustomize SopsSecretGenerator](https://github.com/goabout/kustomize-sopssecretgenerator) for decrypt sops encryption. before start please install [SOPS](https://github.com/getsops/sops) and ensure command of `sops` can be executed on terminal.

then install [Kustomize SopsSecretGenerator](https://github.com/goabout/kustomize-sopssecretgenerator) .&#x20;

```
mkdir $HOME/.config/kustomize/plugin/goabout.com/v1beta1/sopssecretgenerator
wget https://github.com/goabout/kustomize-sopssecretgenerator/releases/download/v1.6.0/SopsSecretGenerator_1.6.0_darwin_arm64 -O $HOME/.config/kustomize/plugin/goabout.com/v1beta1/sopssecretgenerator/SopsSecretGenerator
chmod +x $HOME/.config/kustomize/plugin/goabout.com/v1beta1/sopssecretgenerator/SopsSecretGenerator
```

