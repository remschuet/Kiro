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

# STEP 02 - Dashboard

## Objectif

Créer l'interface principale.

---

## À développer

Dashboard minimaliste futuriste.

---

## Informations affichées

### État Kiro

```text
Online
Offline
Listening
```

### Wake Word

```text
Enabled
Disabled
```

### Moteur IA

```text
Loaded
Not Loaded
```

### Outils

```text
Spotify
Notifications
```

---

## Validation

- [ ] Dashboard fonctionnel
- [ ] Thème sombre appliqué
- [ ] Interface responsive
- [ ] Aucun élément temporaire

---

## Tests

- [ ] Rotation écran
- [ ] Reprise application

---

# STEP 03 - Logging System

## Objectif

Créer le système complet de logs techniques.

---

## À développer

Structure :

```text
Timestamp
Command
Tool
Action
Result
Duration
```

---

## Exemple

```text
20:01:12
Command: next_song
Tool: Spotify
Action: next_song
Result: Success
Duration: 35ms
```

---

## Validation

- [ ] Log créé lors d'une action
- [ ] Log sauvegardé localement
- [ ] Log affichable dans l'application

---

## Tests

- [ ] Ajout log
- [ ] Lecture log
- [ ] Suppression log

---

# STEP 04 - MCP Foundation

## Objectif

Créer le coeur du système.

---

## À développer

### MCP Router

- Validation
- Permissions
- Dispatch

### Interface

```kotlin
interface Tool {
    fun execute()
}
```

---

## Validation

### Cas valide

```text
next_song
```

↓

```text
Action transmise au bon Tool
```

---

### Cas invalide

```text
delete_photos
```

↓

```text
Refus
```

---

## Tests

- [ ] Action autorisée
- [ ] Action inconnue
- [ ] Action vide
- [ ] Tool absent

---

# STEP 05 - Spotify Tool

## Objectif

Première intégration réelle.

---

## À développer

```text
Play
Pause
Next
Previous
Volume Up
Volume Down
```

---

## Validation

### Cas

```text
next_song
```

↓

Musique suivante

---

### Cas

```text
pause
```

↓

Musique en pause

---

## Refus

```text
play_artist
```

↓

Action inconnue

---

## Tests

- [ ] Toutes les actions ajoutent un log
- [ ] Toutes les actions passent par MCP
- [ ] Aucun appel direct depuis l'UI

---

# STEP 06 - Notification Tool

## Objectif

Accéder aux notifications Android.

---

## À développer

Lecture des notifications actives uniquement.

---

## Fonctions

```text
notification_count
who_just_messaged_me
```

---

## Validation

Notifications :

```text
Jean
Marc
```

Commande :

```text
who_just_messaged_me
```

↓

```text
Jean et Marc viennent de vous écrire.
```

---

## Non autorisé

- Lecture historique
- Analyse conversations
- Scan applications

---

## Tests

- [ ] Aucune notification
- [ ] Une notification
- [ ] Plusieurs notifications
- [ ] Applications différentes

---

# STEP 07 - Local Speech To Text

## Objectif

Transformer la voix en texte localement.

---

## À développer

Moteur STT.

Préférence :

```text
Whisper local
```

---

## Validation

Voix :

```text
Kiro chanson suivante
```

↓

Texte :

```text
chanson suivante
```

---

## Tests

- [ ] Micro fonctionnel
- [ ] Silence
- [ ] Bruit ambiant
- [ ] Commandes simples

---

## Attention

Aucune donnée réseau.

---

# STEP 08 - Wake Word

## Objectif

Détection du mot-clé.

---

## Wake Word

```text
Kiro
```

---

## Modes

### Désactivé

App ouverte uniquement.

### Activé

Écoute système.

---

## Validation

```text
Kiro chanson suivante
```

↓

Activation :

```text
chanson suivante
```

---

## Tests

- [ ] Kiro détecté
- [ ] Faux positifs
- [ ] Écran éteint
- [ ] Écran verrouillé

---

## Attention

Limiter l'impact batterie.

---

# STEP 09 - LLM Local

## Objectif

Ajouter l'intelligence locale.

---

## Responsabilités

- Comprendre
- Répondre
- Choisir une action MCP

---

## Ne doit jamais

- Contrôler Android directement
- Exécuter une action
- Accéder aux outils

---

## Exemple

Entrée :

```text
Passe à la chanson suivante
```

Sortie :

```json
{
  "tool": "spotify",
  "action": "next_song"
}
```

---

## Validation

Le LLM doit produire :

- une action valide
- ou un refus

Jamais autre chose.

---

## Tests

- [ ] Commandes Spotify
- [ ] Notifications
- [ ] Commande inconnue
- [ ] Tentative interdite

---

# STEP 10 - End To End Validation

## Objectif

Valider la chaîne complète.

---

## Pipeline

```text
Voix
↓
Wake Word
↓
STT
↓
LLM
↓
MCP
↓
Tool
↓
Action
```

---

## Cas 1

```text
Kiro chanson suivante
```

↓

```text
Spotify -> Next Song
```

---

## Cas 2

```text
Kiro qui vient de m'écrire
```

↓

```text
Notification Tool
↓
Réponse vocale
```

---

## Cas 3

```text
Kiro supprime mes photos
```

↓

```text
Je ne possède pas cette capacité.
```

---

## Validation finale

### Fonctionnel

- [ ] Spotify fonctionne
- [ ] Notifications fonctionnent
- [ ] Wake word fonctionne
- [ ] STT fonctionne
- [ ] LLM fonctionne
- [ ] MCP fonctionne

### Sécurité

- [ ] Aucune action sans MCP
- [ ] Aucune action hors Tool
- [ ] Aucun accès direct système par le LLM

### Confidentialité

- [ ] Aucune donnée envoyée à Internet
- [ ] Fonctionnement hors-ligne
- [ ] Tous les traitements sont locaux

### UX

- [ ] Réponses naturelles
- [ ] Pas de réponses inutiles
- [ ] Interface cohérente

---

# Fin de V1

Le projet est considéré V1 lorsque les 10 étapes sont validées avec succès.
