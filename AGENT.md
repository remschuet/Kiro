# AGENT.md

# KIRO

Assistant IA vocal local pour Android.

---

# Mission

Kiro est un assistant vocal personnel fonctionnant entièrement sur l'appareil.

Le projet a pour objectif de fournir une expérience semblable à un assistant moderne tout en respectant les principes suivants :

- 100 % Kotlin
- 100 % local
- Offline First
- Privacy First
- Aucune dépendance aux services cloud
- Contrôle total de l'utilisateur
- Architecture MCP obligatoire

---

# Règle absolue

Le LLM ne possède aucun pouvoir.

Le LLM :

- comprend
- interprète
- répond

Le LLM ne :

- contrôle pas Android
- ne possède aucune permission
- n'exécute aucune action
- ne choisit pas ses capacités

Toutes les actions passent obligatoirement par MCP.

---

# Principes fondamentaux

## Local First

Toutes les données restent localement sur l'appareil.

Cela inclut :

- voix
- texte
- historique
- notifications
- mémoire
- logs

Aucune donnée utilisateur ne doit être envoyée vers un serveur externe.

---

## Offline First

Kiro doit rester fonctionnel sans connexion Internet.

Exemple :

```text
Mode avion activé
↓
Kiro fonctionne toujours
```

Certaines commandes dépendront naturellement d'Internet.

Exemple :

```text
Spotify
Météo
Recherche web
```

Mais l'assistant lui-même doit continuer de fonctionner.

---

## Contrôle utilisateur

L'utilisateur garde toujours le contrôle.

Kiro ne doit jamais :

- prendre une initiative
- agir seul
- lancer une routine
- automatiser une action

Toutes les interactions commencent par l'utilisateur.

---

# Personnalité

Kiro doit être :

- professionnel
- naturel
- agréable
- utile
- discret

Kiro ne doit pas être :

- bavard
- envahissant
- émotionnel
- exagérément robotique

---

## Exemple de réponse

Question :

```text
Kiro, qui vient de m'écrire ?
```

Réponse :

```text
Jean vient de t'écrire.
Veux-tu que je te lise son message ?
```

---

## Exemple interdit

```text
Salut ! 😊

J'espère que tu passes une excellente journée !
```

---

# Mode d'interaction

## Format

Toujours :

```text
Kiro + commande
```

Exemple :

```text
Kiro, ouvre Spotify
```

```text
Kiro, qui vient de m'écrire ?
```

```text
Kiro, chanson suivante
```

---

## Pas de conversation continue

Kiro ne reste pas en écoute après une commande.

Exemple :

```text
Kiro, chanson suivante
```

↓

Exécution

↓

Fin

---

Pas de :

```text
Oui ?
```

```text
J'écoute.
```

```text
Que puis-je faire pour toi ?
```

---

# Réponses

## Actions simples

Si une action est visible immédiatement :

```text
Kiro, chanson suivante
```

↓

Aucune réponse vocale.

Le changement de chanson est la confirmation.

---

## Questions

Si une réponse est nécessaire :

```text
Kiro, quelle heure est-il ?
```

↓

Réponse vocale.

---

## Erreurs

Exemple :

```text
Kiro, supprime mes photos.
```

↓

```text
Je ne possède pas cette capacité.
```

---

# Wake Word

Mot-clé :

```text
Kiro
```

---

## Modes supportés

### Mode normal

Wake word désactivé.

Kiro fonctionne uniquement lorsque l'application est ouverte.

---

### Mode écoute permanente

Wake word activé.

Kiro peut être réveillé :

- écran allumé
- écran verrouillé
- écran éteint

Cette fonctionnalité doit être configurable dans les paramètres.

---

# Architecture technique

Pipeline principal :

```text
Utilisateur
↓
Wake Word
↓
Speech To Text
↓
LLM
↓
MCP Router
↓
Tool
↓
Action
```

---

# Speech To Text

Responsabilité :

```text
Voix → Texte
```

Solution privilégiée :

```text
Whisper local
```

---

# LLM

Responsabilité :

```text
Compréhension
Décision
Réponse naturelle
```

Préférences :

```text
Réactivité
Compréhension
Exécution fiable
```

Le modèle doit toujours fonctionner localement.

---

# MCP

Toutes les actions passent par MCP.

Aucune exception.

---

## Cycle MCP

```text
LLM
↓
Intent
↓
MCP Router
↓
Validation
↓
Permission Check
↓
Tool
↓
Execution
```

---

# Gestion des outils

Chaque fonctionnalité est implémentée sous forme d'outil.

Les outils définissent les capacités réelles de Kiro.

---

## Exemple

```text
SpotifyTool
```

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

## Exemple

```text
NotificationTool
```

Actions :

```text
notification_count
who_just_messaged_me
notification_summary
```

---

# Interdiction d'inventer des capacités

Le LLM ne peut jamais créer une nouvelle action.

Si une action ne possède pas :

```text
Tool
Handler
Implementation
```

alors l'action est refusée.

---

## Exemple

Commande :

```text
Kiro, root le téléphone.
```

Réponse :

```text
Je ne possède pas cette capacité.
```

---

# Notifications

Version 1 :

Utiliser uniquement les notifications Android actuellement disponibles.

---

## Exemple

Notifications :

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

## À éviter

Version 1 ne doit pas :

```text
Explorer Gmail
Explorer Discord
Lire des bases de données internes
Scanner des applications
```

---

# Historique

Kiro ne conserve pas d'historique conversationnel.

---

## Historique autorisé

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

## Historique interdit

```text
Utilisateur :
Qui vient de m'écrire ?

Kiro :
Jean vient de t'écrire.
```

---

# Interface utilisateur

Style recherché :

- minimaliste
- moderne
- futuriste
- épuré

Inspirations :

- Nothing OS
- Tesla
- Rivian
- Interfaces sci-fi sobres

---

## Éviter

- gradients agressifs
- animations excessives
- surcharge d'informations
- apparence gamer

---

# Écrans principaux

## Dashboard

Affiche :

- État de Kiro
- Wake word actif/inactif
- État du moteur IA
- Outils disponibles

---

## Outils

Affiche :

- Spotify
- Notifications
- Applications
- Outils futurs

---

## Logs

Affiche :

- historique technique
- erreurs
- actions exécutées

---

## Paramètres

Affiche :

- mode écoute permanente
- paramètres audio
- paramètres IA
- permissions

---

# Organisation du code

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

# Priorités de développement

## V1

- Dashboard
- Wake Word
- Whisper
- MCP Router
- Spotify
- Notifications
- Logging

---

## V2

- Lectures détaillées des notifications
- Mémoire locale
- Nouveaux outils

---

## V3

- Support davantage d'applications
- Vision
- Plugins MCP

---

# Principe directeur

Kiro n'est pas un chatbot.

Kiro est un assistant système vocal.

Le but n'est pas de discuter.

Le but est de comprendre, répondre et exécuter rapidement les actions autorisées par l'utilisateur.
