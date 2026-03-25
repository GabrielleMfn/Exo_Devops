# Exo 03-04 — Terraform

## Ce que fait l'exercice

- Exo 03 : fondamentaux Terraform (provider Docker, variables, state, tfvars).
- Exo 04 : modules réutilisables + environnements.

## Commandes d'exécution / test

### Exo 03 (racine terraform)

```bash
cd exo/03-04-terraform
terraform init
terraform plan -var-file="dev.tfvars"
terraform apply -var-file="dev.tfvars" -auto-approve
terraform destroy -var-file="dev.tfvars" -auto-approve
```

### Exo 04 (environnement modulaire)

```bash
cd exo/03-04-terraform/environments/dev
terraform init
terraform plan -var-file="terraform.tfvars" -var="db_password=secret123"
terraform apply -var-file="terraform.tfvars" -var="db_password=secret123" -auto-approve
terraform destroy -var-file="terraform.tfvars" -var="db_password=secret123" -auto-approve
```

## Problèmes techniques rencontrés

J'ai dû gérer une incohérence d'architecture car plusieurs layouts Terraform coexistaient (`basic`, `modules-layout` et layout starter classique). J'ai aussi eu des conflits de ports (`8080` et `5432`) lorsque d'autres conteneurs étaient déjà actifs. Enfin, l'absence de la variable sensible `db_password` bloquait les phases `plan/apply` sur le module database.

## Correctifs appliqués

J'ai conservé les deux approches Terraform (fondamentaux et modulaire) tout en ajoutant les fichiers manquants pour rester compatible avec la structure attendue. J'ai passé explicitement les secrets via `-var` ou `TF_VAR_db_password` pour garantir des exécutions reproductibles. J'ai aussi systématisé `terraform validate` et un `destroy` en fin de test pour repartir proprement.

