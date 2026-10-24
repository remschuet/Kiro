# KIRO - Development Steps

Ce document décrit les étapes officielles nécessaires pour atteindre la V1.

Chaque étape doit être terminée, testée et validée avant de passer à la suivante.

L'objectif est qu'un agent IA puisse prendre une étape, l'implémenter complètement, puis vérifier les critères de validation.

---

# STEP 01 - Foundation

## Objectif

Créer une base de projet saine et maintenable.

---

## À développer

- Architecture MVVM
- Structure des packages
- Navigation de base
- Gestion des erreurs globale
- Logging centralisé
- Système de configuration

---

## Packages minimum

```text
core/
ui/
mcp/
tools/
speech/
wakeword/
settings/
logs/
```

---

## Validation

### Build

- [ ] Le projet compile sans erreur
- [ ] Aucun warning critique

### Architecture

- [ ] Packages créés
- [ ] Structure documentée

### UI

- [ ] Application démarre
- [ ] Écran Dashboard visible

---

## Tests

- [ ] Test de lancement de l'application
- [ ] Test de création du logger

---
