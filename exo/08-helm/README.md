# Exo 08 — Helm

## Ce que fait l'exercice

Packager l'application Kubernetes en chart Helm, gérer les values par environnement, puis installer/mettre à jour la release.

## Commandes d'exécution / test

Depuis `exo` :

```bash
helm lint 08-helm/devops-app-chart
helm template devops-app 08-helm/devops-app-chart -f 08-helm/values-dev.yaml --set secrets.dbPassword=supersecret123
helm install devops-app 08-helm/devops-app-chart -n devops-training --create-namespace -f 08-helm/values-dev.yaml --set secrets.dbPassword=supersecret123
helm list -n devops-training
kubectl get all -n devops-training
helm upgrade devops-app 08-helm/devops-app-chart -n devops-training -f 08-helm/values-prod.yaml --set secrets.dbPassword=supersecret123
helm history devops-app -n devops-training
```

## Problèmes techniques rencontrés

Pendant la réalisation, j'ai rencontré une incohérence entre certains noms de helpers et les templates réellement utilisés dans le chart, ce qui cassait le rendu Helm. J'ai aussi eu un problème de probes qui pointaient sur `/` alors que l'application était prévue pour répondre sur `/health`. Enfin, j'ai eu des erreurs de déploiement quand le mot de passe DB n'était pas injecté explicitement selon l'environnement.

## Correctifs appliqués

J'ai harmonisé les helpers Helm avec les templates du chart pour éliminer les erreurs de rendu. J'ai ensuite aligné toutes les probes sur `/health` pour correspondre au comportement réel de l'application. Pour la configuration sensible, j'ai systématiquement injecté le secret DB au `install/upgrade` avec `--set` afin d'avoir un déploiement reproductible en dev et en prod.