\# Architecture



\# KIRO



Assistant IA vocal local pour Android.



\---



\# Objectif de l'architecture



Créer un système modulaire où :



\- chaque composant a une responsabilité unique

\- le LLM est isolé du système Android

\- les actions passent obligatoirement par un routeur MCP

\- les nouveaux outils peuvent être ajoutés facilement



\---



\# Vue d'ensemble



```text

Utilisateur

&#x20;     │

&#x20;     ▼

&#x20;Wake Word

&#x20;     │

&#x20;     ▼

&#x20;Speech To Text

&#x20;     │

&#x20;     ▼

&#x20;  LLM Engine

&#x20;     │

&#x20;     ▼

&#x20; MCP Router

&#x20;     │

&#x20;┌────┼────────────┬────────────┐

&#x20;▼    ▼            ▼            ▼

Spotify Notifications Applications System

```



\---



\# Couche UI



Responsabilité :



\- affichage

\- dashboard

\- historique

\- paramètres



La couche UI ne doit jamais exécuter directement une action système.



\---



\# Couche Voice



Responsabilité :



\- gestion microphone

\- écoute vocale

\- envoi au STT



Modules :



```text

VoiceManager

MicrophoneManager

```



\---



\# Couche Wake Word



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



\---



\# Couche STT



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



\---



\# Couche LLM



Responsabilité :



Comprendre l'intention utilisateur.



Ne doit jamais :



\- accéder au système

\- contrôler Android

\- gérer des permissions



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



\---



\# Couche MCP



Responsabilité :



Point d'entrée unique des actions.



Toutes les demandes du modèle doivent passer ici.



\---



\# MCP Router



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

&#x20; "tool":"spotify",

&#x20; "action":"next\_song"

}

```



\---



\# Cycle MCP



```text

LLM

&#x20;↓

MCP Request

&#x20;↓

Validation

&#x20;↓

Permission Check

&#x20;↓

Tool Execution

&#x20;↓

Response

```



\---



\# Structure MCP



```text

mcp/

│

├── router/

├── validation/

├── permissions/

├── request/

└── response/

```



\---



\# Tool System



Chaque outil doit implémenter :



```kotlin

interface Tool {

&#x20;   fun execute()

}

```



\---



\# Spotify Tool



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



\---



\# Notification Tool



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



\---



\# System Tool



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



\---



\# App Tool



Responsabilités :



```text

Ouvrir application

```



Classe :



```kotlin

AppTool

```



\---



\# Permissions Engine



But :



Limiter les actions autorisées.



Niveaux :



\---



\## Niveau 1



Exécution immédiate.



```text

play

pause

battery\_status

time

```



\---



\## Niveau 2



Confirmation requise.



```text

call\_contact

send\_sms

share\_location

```



\---



\## Niveau 3



Bloqué.



```text

factory\_reset

delete\_files

install\_apk

```



\---



\# Mémoire



Responsabilité :



Stockage local.



Catégories :



```text

Préférences

Historique

Mémoire utilisateur

```



\---



\# Logging



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

next\_song

success

45ms

```



\---



\# MVVM



Architecture globale :



```text

UI

&#x20;↓

ViewModel

&#x20;↓

Repository

&#x20;↓

Service

&#x20;↓

Tool / Engine

```



\---



\# Organisation du code



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



\---



\# V1



Modules obligatoires :



\- Dashboard

\- Wake Word

\- STT

\- MCP Router

\- Spotify Tool

\- Logging



\---



\# V2



Modules optionnels :



\- Notifications

\- Mémoire persistante

\- Personnalité configurable



\---



\# V3



Modules avancés :



\- Vision

\- Automatisation

\- Routines

\- Plugins MCP utilisateur



\---



\# Règle fondamentale



Le LLM n'a aucun accès au système.



Le LLM produit uniquement une intention.



Le routeur MCP décide.



Le Tool exécute.



L'utilisateur conserve toujours le contrôle.

