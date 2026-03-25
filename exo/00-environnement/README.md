# Exo 00 — Vérification d'environnement

## Ce que fait l'exercice

Valider que l'environnement local est prêt pour toute la suite (Docker, Kubernetes, Terraform, Helm, Ansible, GitHub Actions).

## Commandes d'exécution / test

```bash
git --version
docker --version
docker compose version
kubectl version --client
terraform --version
helm version --short
```

Sur Windows (Ansible recommandé via Linux/WSL2) :

```powershell
wsl --list --verbose
```

Tests rapides :

```bash
docker run --rm hello-world
kubectl cluster-info
```

## Problèmes techniques rencontrés

Au départ, il me manquait plusieurs outils indispensables (`helm`, `kind`, `ansible`), ce qui bloquait l'enchaînement des exercices. J'ai ensuite rencontré des erreurs Docker de type `unexpected EOF` pendant la création du cluster `kind`. En parallèle, j'ai rencontré une saturation du disque `C:`, qui dégradait fortement la stabilité de mon Docker Desktop.

## Correctifs appliqués

J'ai installé `kind` et `helm` en version portable sur `D:\DevOpsTools\bin` pour éviter d'aggraver la charge sur mon Disque `C:`. J'ai déplacé le stockage Docker vers `D:\DockerData`, puis redémarré Docker Desktop avant de relancer les opérations. Pour Ansible, j'ai retenu l'approche recommandée hors Windows natif (WSL2), avec une alternative conteneur pour la démonstration.
