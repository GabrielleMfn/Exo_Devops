# Exo 01 — App Node.js + tests

## Ce que fait l'exercice

Créer l'application fil rouge (Node.js), exposer les endpoints de santé/métriques, et valider le socle de tests unitaires.

## Commandes d'exécution / test

Depuis `exo/app` :

```bash
npm ci
npm test
npm start
```

Docker image utilisée par les exos 06/07/08 :

```bash
docker build -t devops-app:1.0.0 .
```

## Endpoints

- `/` : réponse JSON simple
- `/health` : healthcheck HTTP pour Docker/Kubernetes
- `/metrics` : métriques Prometheus en texte brut

## Problèmes techniques rencontrés

J'ai constaté une incohérence entre l'endpoint réellement prévu pour les probes (`/health`) et certaines configurations qui testaient `/`. J'ai aussi reçu des alertes de vulnérabilités Docker (niveau HIGH) liées à l'image de base système, alors que le code applicatif lui-même restait simple et maîtrisé.

## Correctifs appliqués

J'ai aligné les probes de l'ensemble des exos sur `/health` pour supprimer les faux négatifs de readiness/liveness. J'ai maintenu une exécution non-root dans l'image de production pour renforcer la sécurité. Enfin, j'ai gardé une validation systématique avec `npm test` avant chaque build ou déploiement.

## Note sur l'alerte de vulnérabilités Docker

L'alerte "11 high vulnerabilities" détectée sur le `Dockerfile` vient principalement de l'image de base (`node:20-alpine`) et de paquets système embarqués, pas d'une faille introduite par le code de l'exercice.

Dans le cadre de ce lab, ce point n'empêche pas l'exécution ni la validation fonctionnelle des exercices. En revanche, en contexte production, je traiterais cette alerte en mettant à jour l'image de base (tag/pinning plus récent), en rebuildant régulièrement et en appliquant une politique de scan continue en CI.
