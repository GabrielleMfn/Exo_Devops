# Exo 07 — Kubernetes

## Ce que fait l'exercice

Déployer une application multi-tiers (app + PostgreSQL) avec ConfigMap, Secret, PVC, Deployments et Services.

## Commandes d'exécution / test

Depuis `exo` :

```bash
kind create cluster --name devops-training --image kindest/node:v1.30.0 --wait 180s
kubectl config use-context kind-devops-training
docker build -t devops-app:1.0.0 app
kind load docker-image devops-app:1.0.0 --name devops-training
kubectl create namespace devops-training
kubectl create secret generic app-secrets -n devops-training --from-literal=DB_USER=appuser --from-literal=DB_PASSWORD=supersecret123
kubectl apply -f 07-kubernetes/configmap.yaml
kubectl apply -f 07-kubernetes/postgres.yaml
kubectl apply -f 07-kubernetes/app.yaml
kubectl get pods -n devops-training
kubectl port-forward -n devops-training svc/devops-app-svc 8080:80
curl http://localhost:8080/health
```

## Problèmes techniques rencontrés

J'ai eu une instabilité au moment de créer le cluster `kind` avec une version de node trop récente dans mon contexte Docker/WSL. Après certains redémarrages Docker, le contexte Kubernetes n'était plus correctement positionné, ce qui donnait des erreurs API côté `kubectl`. J'ai également perdu du temps sur une commande avec un flag non supporté (`--max-wait`) qui bloquait la vérification des pods.

## Correctifs appliqués

J'ai figé la création du cluster sur `kindest/node:v1.30.0`, ce qui a stabilisé l'initialisation. Ensuite, j'ai refait une recréation propre du cluster et remis explicitement le contexte `kind-devops-training` avant chaque déploiement. Pour le debug, j'ai gardé une vérification progressive avec `kubectl get pods`, `describe` et `logs`, sans utiliser de flags invalides.
