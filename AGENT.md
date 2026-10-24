\# AGENT.md



\# KIRO



Assistant IA vocal local pour Android.



\---



\# Vision du projet



Kiro est un assistant IA personnel fonctionnant entièrement sur Android.



Objectifs :



\- 100 % Kotlin

\- Fonctionnement local prioritaire

\- Aucun cloud requis

\- Respect de la vie privée

\- Architecture MCP sécurisée

\- Interface minimaliste et futuriste

\- Optimisé pour Pixel mais compatible Android moderne



Kiro ne doit jamais avoir un accès illimité au système.



Le modèle IA ne possède aucun pouvoir direct.



Toutes les actions transitent par un routeur MCP contrôlé par l'application.



\---



\# Philosophie produit



Le projet doit donner l'impression :



\- d'un système d'exploitation intelligent

\- d'une IA calme

\- d'une technologie discrète

\- d'un produit haut de gamme



Inspirations :



\- Nothing OS

\- Rivian UI

\- Tesla UI

\- Interstellar

\- Iron Man (Jarvis)

\- Android Material 3



Ne pas copier directement ces interfaces.



\---



\# Principes techniques



\## Langage



Kotlin uniquement.



Pas de Python.



Pas de NodeJS.



Pas de backend externe obligatoire.



\---



\## Architecture



Architecture modulaire.



Chaque fonctionnalité doit être isolée.



Exemple :



```text

core

voice

wakeword

notification

spotify

llm

memory

mcp

ui

```



\---



\# Structure recommandée



```text

app/

│

├── core/

│

├── voice/

│

├── wakeword/

│

├── llm/

│

├── mcp/

│

├── memory/

│

├── notification/

│

├── spotify/

│

├── permissions/

│

├── settings/

│

└── ui/

```



\---



\# Design System



\## Thème général



Futuriste.



Minimaliste.



Sobre.



Aucune surcharge visuelle.



\---



\## Couleurs



Fond principal :



```text

\#090909

```



Fond secondaire :



```text

\#121212

```



Fond cartes :



```text

\#1A1A1A

```



Accent principal :



```text

\#00E5FF

```



Accent secondaire :



```text

\#64FFDA

```



Texte :



```text

\#FFFFFF

```



Texte secondaire :



```text

\#B0BEC5

```



Erreur :



```text

\#FF5252

```



\---



\## Style



Préférer :



\- beaucoup d'espace

\- peu de texte

\- cartes simples

\- animations douces

\- transparence légère



Éviter :



\- couleurs agressives

\- effets flashy

\- gradients excessifs

\- interfaces denses



\---



\# Interface utilisateur



L'application doit sembler légère.



Maximum 5 écrans principaux.



\---



\## Dashboard



Écran principal.



Contient :



\- État de Kiro

\- Écoute active

\- Batterie

\- Dernière commande

\- Raccourcis rapides



\---



\## Historique



Liste des interactions.



Format :



```text

Utilisateur

↓

Réponse IA

↓

Action réalisée

```



\---



\## Outils



Liste des outils MCP disponibles.



Exemples :



```text

Spotify

Notifications

Batterie

Applications

```



\---



\## Mémoire



Affichage de ce que Kiro retient.



L'utilisateur doit pouvoir :



\- consulter

\- modifier

\- supprimer



\---



\## Paramètres



Configuration complète.



\---



\# UX



L'application doit toujours inspirer :



```text

Confiance

Simplicité

Fluidité

Contrôle

```



\---



\# Animations



Animations courtes.



Durée cible :



```text

150ms à 300ms

```



Jamais plus de :



```text

500ms

```



\---



\# IA



L'IA ne doit jamais :



\- exécuter directement du code

\- accéder directement au système

\- modifier des permissions

\- installer des applications



\---



\# Architecture MCP



Toutes les actions transitent par :



```text

Utilisateur

↓

Voice

↓

LLM

↓

MCP Router

↓

Tool

```



\---



\# Exemple



Utilisateur :



```text

Kiro, chanson suivante.

```



LLM :



```json

{

&#x20;   "action":"next\_song"

}

```



Router :



```text

Validation

```



Tool :



```text

SpotifyTool.next()

```



\---



\# Outils autorisés V1



Spotify :



```text

play

pause

next

previous

volume

```



Notifications :



```text

read\_notifications

notification\_summary

```



Système :



```text

battery\_status

current\_time

current\_date

```



Applications :



```text

open\_app

```



\---



\# Outils interdits



Toujours refusés :



```text

factory\_reset

delete\_files

root\_device

install\_apk

adb\_command

grant\_permissions

```



\---



\# Voice Stack



Version cible :



```text

Wake Word

↓

Speech To Text

↓

LLM

↓

Text To Speech

```



\---



\# Performance



Objectif :



Temps de réponse inférieur à :



```text

2 secondes

```



pour une commande locale.



\---



\# Logging



Toutes les actions MCP doivent être enregistrées.



Chaque log doit inclure :



```text

Date

Commande

Action

Succès/Échec

Durée

```



\---



\# Sécurité



Aucune action critique sans validation utilisateur.



Toujours privilégier :



```text

Privacy First

Local First

User First

```



\---



\# Qualité du code



Règles :



\- fonctions courtes

\- classes uniques

\- architecture claire

\- commentaires uniquement si nécessaire

\- éviter le code dupliqué



Appliquer :



```text

SOLID

Clean Architecture

MVVM

```



\---



\# Versions



\## v0.1



\- Interface

\- Dashboard

\- Wake Word

\- Architecture MCP



\## v0.2



\- Spotify

\- Notifications

\- Historique



\## v0.3



\- Mémoire locale



\## v0.4



\- LLM local



\## v1.0



Assistant complet utilisable quotidiennement.



\---



\# Règle absolue



Le modèle IA ne possède aucun pouvoir.



Le modèle propose.



Kiro décide.



L'utilisateur contrôle.

