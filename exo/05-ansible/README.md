# Exo 05 — Ansible

## Ce que fait l'exercice

Configurer des "serveurs" simulés par conteneurs Docker avec playbooks et roles, puis démontrer l'idempotence.

## Commandes d'exécution / test

Depuis `exo/05-ansible` :

```bash
docker compose up -d
ansible-galaxy collection install -r requirements.yml
ansible all -i inventory.yml -m ping
ansible-playbook -i inventory.yml playbook-base.yml
ansible-playbook -i inventory.yml playbook-nginx.yml
ansible-playbook -i inventory.yml site.yml
ansible-playbook -i inventory.yml site.yml
```

Nettoyage :

```bash
docker compose down
```

## Problèmes techniques rencontrés

J'ai rencontré une limite d'environnement: Ansible n'était pas disponible en natif sur Windows dans ma configuration. J'ai aussi eu une incohérence de connexion tant que la collection `community.docker` n'était pas installée et correctement utilisée avec l'inventaire. Enfin, le comportement du handler Nginx n'était pas suffisamment robuste avec un lancement en arrière-plan.

## Correctifs appliqués

J'ai gardé une approche compatible avec WSL2/Linux pour exécuter Ansible dans de bonnes conditions. J'ai aligné l'inventaire sur `community.docker.docker` et vérifié la résolution des hôtes. J'ai également adapté le handler Nginx vers un comportement plus stable en reload/start. Pour l'objectif pédagogique, j'ai conservé la démonstration d'idempotence avec un second run attendu sans changement.
