# KIRO

Assistant IA vocal local pour Android.

Kiro est un assistant vocal conçu pour fonctionner directement sur l'appareil, sans dépendance obligatoire à un service cloud.

L'objectif est de fournir une expérience simple, rapide et privée tout en gardant un contrôle total sur les actions autorisées.

---

## Principes

- 100 % Kotlin
- 100 % local
- Offline First
- Privacy First
- Wake word configurable
- Architecture MCP sécurisée
- Contrôle total de l'utilisateur

---

## Fonctionnalités prévues

### V1

- Wake Word
- Speech To Text local
- LLM local
- Spotify
- Notifications Android
- Logging technique

Exemples :

```text
Kiro, chanson suivante
```

```text
Kiro, qui vient de m'écrire ?
```

```text
Kiro, ouvre Spotify
```

---

## Architecture

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

Le LLM ne possède jamais un accès direct au système Android.

Toutes les actions sont validées par le routeur MCP.

---

## Philosophie

Le modèle IA ne possède aucun pouvoir.

Il peut uniquement proposer une action.

Le système décide si cette action est autorisée ou non.

---

## Documentation

- [AGENT.md](AGENT.md)
- [Architecture](docs/ARCHITECTURE.md)
- [Roadmap](docs/ROADMAP.md)

---

## État du projet

🚧 En développement

---

## Licence

MIT
