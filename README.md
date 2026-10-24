# 🤖 Bob

> Un assistant IA vocal local pour Android, privé, extensible et entièrement contrôlé par l'utilisateur.

https://img.shields.io/badge/Platform-Android-green
https://img.shields.io/badge/Kotlin-Android-blue
https://img.shields.io/badge/LLM-Local-orange
https://img.shields.io/badge/Offline-Yes-success
https://img.shields.io/badge/Privacy-First-red

---

## 🎯 Vision

**Bob** est un assistant vocal personnel qui fonctionne directement sur un téléphone Android sans dépendre du cloud.

Contrairement aux assistants traditionnels, Bob :

- ✅ Fonctionne hors-ligne
- ✅ Exécute un LLM local
- ✅ Respecte la vie privée
- ✅ N'envoie aucune donnée sur Internet
- ✅ Utilise une architecture sécurisée basée sur des MCP (Model Context Protocol)
- ✅ Ne possède que les permissions explicitement définies par l'utilisateur

L'objectif est de créer une alternative personnelle à :

- Google Assistant
- Siri
- Alexa
- ChatGPT Voice

tout en conservant un contrôle total sur les données et les actions autorisées.

---

# ✨ Fonctionnalités prévues

## 🎤 Activation vocale

Bob est en écoute passive.

Lorsqu'il entend :

```text
Bob
```

il se réveille et attend une commande.

Exemples :

```text
Bob, passe la chanson suivante.
Bob, ouvre Spotify.
Bob, qui m'a écrit aujourd'hui ?
Bob, quel est mon niveau de batterie ?
```

---

## 🧠 Intelligence artificielle locale

Le modèle tourne directement sur l'appareil.

Exemples de modèles compatibles :

- Phi-3 Mini
- Gemma
- Qwen
- Llama

Aucune connexion Internet n'est nécessaire.

---

## 🗣️ Reconnaissance vocale

Transformation :

```text
Voix → Texte
```

Technologies envisagées :

- Whisper.cpp
- Vosk

---

## 🔊 Réponse vocale

Transformation :

```text
Texte → Voix
```

Technologies possibles :

- Android TTS
- Piper TTS

---

## 🔔 Lecture des notifications

Bob peut lire ou résumer :

- SMS
- WhatsApp
- Messenger
- Discord
- Gmail
- Notifications Android

Exemple :

```text
Bob, ai-je reçu des messages ?
```

Réponse :

```text
Tu as reçu 2 messages WhatsApp et 1 courriel.
```

---

## 🎵 Contrôle Spotify

Actions supportées :

```text
Play
Pause
Chanson suivante
Chanson précédente
Volume
```

Exemple :

```text
Bob, chanson suivante.
```

---

# 🔒 Sécurité

## Principe fondamental

Le modèle IA ne contrôle jamais directement Android.

Il ne possède aucun accès :

```text
❌ Root
❌ ADB
❌ Fichiers système
❌ Applications
❌ Permissions Android
```

Le modèle ne peut qu'émettre une demande d'action.

---

## Architecture MCP

```text
Utilisateur
     │
     ▼
Wake Word ("Bob")
     │
     ▼
Speech To Text
     │
     ▼
LLM Local
     │
     ▼
MCP Router
     │
 ┌───┼──────────────┐
 ▼   ▼              ▼
Spotify    Notifications    Système
```

---

## Exemple d'exécution

Commande utilisateur :

```text
Bob, passe la chanson suivante.
```

Le LLM produit :

```json
{
  "action": "next_song"
}
```

Puis le routeur :

```kotlin
executeAction("next_song")
```

Et uniquement si cette action est autorisée.

---

# 🛠️ Outils disponibles

## Audio

```text
play_music
pause_music
next_song
previous_song
set_volume
```

---

## Notifications

```text
read_notifications
last_notification
notification_summary
```

---

## Informations système

```text
battery_status
wifi_status
current_time
current_date
```

---

## Applications

```text
open_spotify
open_camera
open_maps
open_settings
```

---

# 🚫 Actions interdites

Même si le modèle tente de les demander :

```text
factory_reset
install_apk
delete_photos
format_storage
root_device
grant_permission
```

Le système répond :

```json
{
  "error": "action_not_allowed"
}
```

---

# 🏗️ Architecture

## Wake Word

Détection du mot :

```text
Bob
```

Solutions possibles :

- Porcupine
- OpenWakeWord
- Mycroft Precise

---

## Speech To Text

```text
Voix → Texte
```

Solutions :

- Whisper.cpp
- Vosk

---

## LLM

```text
Compréhension des commandes
Décision des actions
Réponses utilisateur
```

Moteurs possibles :

- llama.cpp
- MLC LLM

Modèles :

- Phi-3 Mini
- Gemma 2B
- Qwen 2.5 3B
- Llama 3

---

## Text To Speech

```text
Texte → Voix
```

Solutions :

- Android TTS
- Piper

---

## Router MCP

Responsabilités :

- Validation des actions
- Contrôle des permissions
- Journalisation
- Exécution sécurisée

---

# 📁 Structure du projet

```text
bob/
│
├── app/
│
├── wakeword/
│   ├── detector
│   └── models
│
├── speech/
│   ├── whisper
│   └── stt
│
├── llm/
│   ├── models
│   └── prompts
│
├── mcp/
│   ├── router
│   ├── permissions
│   └── tools
│
├── spotify/
│
├── notifications/
│
├── tts/
│
└── docs/
```

---

# 🔐 Niveaux de permissions

## Niveau 1 - Exécution automatique

```text
next_song
pause_song
battery_status
current_time
```

---

## Niveau 2 - Confirmation requise

```text
send_sms
call_contact
share_location
```

Exemple :

```text
Veux-tu vraiment envoyer ce message ?
```

---

## Niveau 3 - Interdit

```text
factory_reset
delete_files
install_app
```

Toujours refusé.

---

# 🚀 Roadmap

## v0.1

- [ ] Wake word "Bob"
- [ ] Whisper local
- [ ] LLM local
- [ ] TTS
- [ ] Spotify

## v0.2

- [ ] Notifications Android
- [ ] Gestion du volume
- [ ] Historique des interactions

## v0.3

- [ ] Mémoire utilisateur locale
- [ ] Personnalité personnalisable
- [ ] Actions automatisées

## v1.0

- [ ] Assistant complètement autonome
- [ ] Écosystème MCP extensible
- [ ] Marketplace d'outils locaux
- [ ] Interface graphique complète

---

# 📜 Philosophie
