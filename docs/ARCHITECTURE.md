# Architecture

# KIRO

Assistant IA vocal local pour Android.

---

# Vision

Kiro est un assistant vocal personnel fonctionnant entièrement localement sur Android.

Principes fondamentaux :

- 100 % Kotlin
- 100 % local
- Offline First
- Privacy First
- Aucune donnée utilisateur envoyée vers un serveur externe
- Le LLM ne possède aucun pouvoir direct
- Toutes les actions passent par MCP

---

# Objectifs

Créer un assistant capable de :

```text
"Kiro, chanson suivante."
"Kiro, qui vient de m'écrire ?"
"Kiro, ouvre Spotify."
"Kiro, quelle heure est-il ?"
```

tout en gardant un contrôle total sur :

- les permissions
- les actions disponibles
- les données utilisateur

---

# Principes de conception

## Aucune autonomie

Kiro n'agit jamais seul.

Kiro ne possède pas :

- routines automatiques
- automatisations
- déclencheurs
- suggestions spontanées

Toutes les interactions commencent par :

```text
Utilisateur
↓
"Kiro"
↓
Commande
```

---

## Local uniquement

Tous les traitements doivent être effectués sur le téléphone.

```text
Microphone
STT
LLM
Mémoire
Historique
Notifications
```

restent sur l'appareil.

Aucune transcription ou conversation ne doit être envoyée vers un serveur distant.

---

## Contrôle absolu

Le LLM ne décide jamais de ce qu'il peut faire.

Le développeur définit les capacités autorisées.

Exemple :

```text
"Kiro, supprime mes photos."
```

↓

```text
Action inexistante
```

↓

```text
Je ne possède pas cette capacité.
```

---

# Architecture globale

```text
Utilisateur
      │
      ▼
 Wake Word
      │
      ▼
 Speech To Text
      │
      ▼
   LLM Engine
      │
      ▼
  MCP Router
      │
 ┌────┼───────────────┐
 ▼    ▼               ▼
Tools Permissions  Logging
```

---

# Pipeline complet

```text
Utilisateur
↓
"Kiro, chanson suivante"
↓
Wake Word
↓
Speech To Text
↓
Texte
↓
LLM
↓
Intention
↓
MCP Router
↓
Tool
↓
Exécution
```

---

# Couche Wake Word

Responsabilité :

Détecter l'appel de Kiro.

Exemple :

```text
Kiro, chanson suivante.
```

---

## Modes

### Mode désactivé

Kiro écoute uniquement lorsque l'application est ouverte.

### Mode activé

Kiro écoute même :

```text
Écran verrouillé
Écran éteint
Téléphone dans la poche
```

Cette option doit être configurable par l'utilisateur.

---

# Couche Speech To Text

Responsabilité :

Convertir la voix en texte.

Exemple :

```text
"Kiro, ouvre Spotify."
```

↓

```text
ouvre spotify
```

---

## Interface

```kotlin
interface SpeechToTextEngine
```

---

## Implémentation prévue

```text
Whisper
```

---

# Couche LLM

Responsabilité :

Comprendre la commande utilisateur.

Le LLM :

✅ comprend

✅ interprète

✅ répond

Le LLM :

❌ n'accède pas au système

❌ ne contrôle pas Android

❌ ne choisit pas ses permissions

❌ n'exécute aucune action

---

## Interface

```kotlin
interface LLMEngine
```

---

# Couche MCP

MCP est le point d'entrée unique pour toutes les actions.

Aucune action système ne peut contourner cette couche.

---

# MCP Router

Responsabilités :

```text
Validation
Permissions
Dispatch
Execution
Logging
```

---

## Exemple

Commande :

```text
"Kiro, chanson suivante."
```

Résultat LLM :

```json
{
  "tool": "spotify",
  "action": "next_song"
}
```

Le routeur :

```text
Valide
↓
Vérifie les permissions
↓
Exécute
↓
Journalise
```

---

# Cycle MCP

```text
LLM
↓
MCP Request
↓
Validation
↓
Permission Check
↓
Tool Execution
↓
Response
```

---

# Structure MCP

```text
mcp/
│
├── router/
├── request/
├── validation/
├── permissions/
└── response/
```

---

# Tool System

Chaque fonctionnalité est un outil.

---

## Interface

```kotlin
interface Tool {
    fun execute()
}
```

---

# V1 - Outils autorisés

## Spotify

Actions :

```text
play
pause
next_song
previous_song
volume_up
volume_down
```

---

## Notifications

Actions :

```text
get_notifications
who_just_messaged_me
notification_count
```

---

## Système

Actions :

```text
battery_status
current_time
current_date
```

---

## Applications

Actions :

```text
open_app
```

---

# Vision long terme

L'architecture doit permettre plus tard :

```text
Spotify
YouTube Music
Discord
Maps
Navigation
Autres applications
```

sans modifier MCP.

---

# Gestion des notifications

Version 1 :

Kiro utilise uniquement les notifications Android actuellement visibles.

Exemple :

```text
WhatsApp - Jean
Discord - Marc
```

Commande :

```text
Kiro, qui vient de m'écrire ?
```

Réponse :

```text
Jean et Marc viennent de t'écrire.
```

---

## Pas de lecture directe

Version 1 ne doit pas :

```text
Explorer Gmail
Explorer Discord
Explorer Messenger
Explorer WhatsApp
```

en dehors des notifications exposées par Android.

---

# Réponses utilisateur

## Action simple

Commande :

```text
Kiro, chanson suivante.
```

Réponse :

```text
Aucune réponse vocale.
```

Le changement de chanson constitue déjà la confirmation.

---

## Question

Commande :

```text
Kiro, qui vient de m'écrire ?
```

Réponse :

```text
Jean vient de t'envoyer un message.
Veux-tu que je te le lise ?
```

---

## Erreur

Commande :

```text
Kiro, supprime mes photos.
```

Réponse :

```text
Je ne possède pas cette capacité.
```

---

# Personnalité

Kiro doit être :

```text
Professionnel
Naturel
Serviable
Discret
```

Kiro peut proposer un suivi utile.

Exemple :

```text
Tu as reçu un message de Jean.
Veux-tu que je te le lise ?
```

Kiro ne doit pas :

```text
Parler inutilement
Interrompre l'utilisateur
Agir comme un compagnon virtuel
```

---

# Historique

Aucun historique conversationnel.

---

## Historique conservé

Logs techniques uniquement.

Exemple :

```text
20:04:12
Command: next_song
Tool: Spotify
Result: Success
Duration: 32ms
```

---

## Historique refusé

```text
Utilisateur : Qui m'a écrit ?

Kiro : Jean et Marc t'ont écrit aujourd'hui.
```

---

# Logging

Toutes les actions MCP doivent être journalisées.

Structure :

```text
Timestamp
Commande
Tool
Action
Résultat
Durée
```

---

# MVVM

Architecture globale :

```text
UI
↓
ViewModel
↓
Repository
↓
Service
↓
Tool
```

---

# Organisation du projet

```text
app/src/main/java/com/remi/kiro/

├── core/
├── ui/
├── wakeword/
├── speech/
├── llm/
├── mcp/
├── permissions/
├── tools/
│   ├── spotify/
│   ├── notifications/
│   ├── system/
│   └── apps/
├── logs/
└── settings/
```

---

# Sécurité

Principe fondamental :

```text
Le LLM n'a aucun pouvoir.
```

Le LLM produit une intention.

Le MCP Router décide.

Le Tool exécute.

L'utilisateur conserve toujours le contrôle.
