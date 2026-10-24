# Architecture

# KIRO

Assistant IA vocal local pour Android.

---

# Objectif de l'architecture

Créer un système modulaire où :

- chaque composant a une responsabilité unique
- le LLM est isolé du système Android
- les actions passent obligatoirement par un routeur MCP
- les nouveaux outils peuvent être ajoutés facilement

---

# Vue d'ensemble

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
 ┌────┼────────────┬────────────┐
 ▼    ▼            ▼            ▼
Spotify Notifications Applications System
```

---

# Couche UI

Responsabilité :

- affichage
- dashboard
- historique
- paramètres

La couche UI ne doit jamais exécuter directement une action système.

---

# Couche Voice

Responsabilité :

- gestion microphone
- écoute vocale
- envoi au STT

Modules :

```text
VoiceManager
MicrophoneManager
```

---

# Couche Wake Word

Responsabilité :

Détection du mot-clé :

```text
Kiro
```

Modules :

```text
WakeWordService
WakeWordEngine
```

---

# Couche STT

Responsabilité :

Convertir :

```text
Voix → Texte
```

Interface :

```kotlin
interface SpeechToTextEngine
```

Implémentations futures :

```text
Whisper
Gemini Nano
Autres
```

---

# Couche LLM

Responsabilité :

Comprendre l'intention utilisateur.

Ne doit jamais :

- accéder au système
- contrôler Android
- gérer des permissions

Rôle unique :

```text
Comprendre
Décider
Répondre
```

Interface :

```kotlin
interface LLMEngine
```

---

# Couche MCP

Responsabilité :

Point d'entrée unique des actions.

Toutes les demandes du modèle doivent passer ici.

---

# MCP Router

Responsable de :

```text
Validation
Permissions
Dispatch
Logs
Exécution
```

Exemple :

```json
{
  "tool": "spotify",
  "action": "next_song"
}
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
├── validation/
├── permissions/
├── request/
└── response/
```

---

# Tool System

Chaque outil doit implémenter :

```kotlin
interface Tool {
    fun execute()
}
```

---

# Spotify Tool

Responsabilités :

```text
Play
Pause
Next
Previous
Volume
```

Classe :

```kotlin
SpotifyTool
```

---

# Notification Tool

Responsabilités :

```text
Lire
Résumer
Lister
```

Classe :

```kotlin
NotificationTool
```

---

# System Tool

Responsabilités :

```text
Heure
Date
Batterie
Réseau
```

Classe :

```kotlin
SystemTool
```

---

# App Tool

Responsabilités :

```text
Ouvrir application
```

Classe :

```kotlin
AppTool
```

---

# Permissions Engine

But :

Limiter les actions autorisées.

Niveaux :

---

## Niveau 1

Exécution immédiate.

```text
play
pause
battery_status
time
```

---

## Niveau 2

Confirmation requise.

```text
call_contact
send_sms
share_location
```

---

## Niveau 3

Bloqué.

```text
factory_reset
delete_files
install_apk
```

---

# Mémoire

Responsabilité :

Stockage local.

Catégories :

```text
Préférences
Historique
Mémoire utilisateur
```

---

# Logging

Toutes les actions doivent produire un log.

Structure :

```text
Timestamp
Module
Commande
Résultat
Durée
```

Exemple :

```text
20:01:54
SpotifyTool
next_song
success
45ms
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
Tool / Engine
```

---

# Organisation du code

```text
app/src/main/java/com/remi/kiro/

├── core/
├── ui/
├── voice/
├── wakeword/
├── llm/
├── mcp/
├── tools/
│   ├── spotify/
│   ├── notifications/
│   ├── system/
│   └── apps/
├── memory/
├── settings/
└── logs/
```

---

# V1

Modules obligatoires :

- Dashboard
- Wake Word
- STT
- MCP Router
- Spotify Tool
- Logging

---

# V2

Modules optionnels :

- Notifications
- Mémoire persistante
- Personnalité configurable

---

# V3

Modules avancés :

- Vision
- Automatisation
- Routines
- Plugins MCP utilisateur

---

# Règle fondamentale

Le LLM n'a aucun accès au système.

Le LLM produit uniquement une intention.

Le routeur MCP décide.

Le Tool exécute.

L'utilisateur conserve toujours le contrôle.
