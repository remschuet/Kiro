\# Model Context Protocol



Ce document décrit comment Kiro autorise ou refuse une action.



Le MCP est le seul composant autorisé à exécuter une action système.



\---



\# Philosophie



Le LLM n'a aucun pouvoir.



Le LLM :



\- comprend

\- interprète

\- répond



Le LLM ne :



\- contrôle pas Android

\- exécute des actions

\- choisit ses permissions

\- décide de ce qui est autorisé



\---



\# Flux général



```text

Utilisateur

↓

Whisper

↓

LLM

↓

Intent

↓

MCP Router

↓

Validation

↓

Tool

↓

Action

```



\---



\# Exemple



Commande :



```text

Kiro, chanson suivante

```



Le LLM produit :



```json

{

&#x20; "tool": "spotify",

&#x20; "action": "next\_song"

}

```



Le MCP valide :



```text

spotify existe ?

↓

oui



next\_song existe ?

↓

oui



handler existe ?

↓

oui

```



Puis :



```text

Execution

```



\---



\# Règle fondamentale



Une action est autorisée uniquement si :



```text

Tool existant

\+

Action enregistrée

\+

Handler implémenté

```



Sinon :



```text

Refus

```



\---



\# Tool Registry



Le registre représente les capacités réelles de Kiro.



\---



\## Exemple



```kotlin

object ToolRegistry {



&#x20;   val tools = mapOf(



&#x20;       "spotify" to listOf(

&#x20;           "play",

&#x20;           "pause",

&#x20;           "next\_song",

&#x20;           "previous\_song"

&#x20;       ),



&#x20;       "notifications" to listOf(

&#x20;           "who\_just\_messaged\_me",

&#x20;           "notification\_count"

&#x20;       )

&#x20;   )

}

```



\---



\# Rôle du Registry



Le Registry est la source de vérité.



Si une action n'est pas présente :



```text

Elle n'existe pas.

```



Même si le LLM la demande.



\---



\# Exemple accepté



Le LLM produit :



```json

{

&#x20; "tool": "spotify",

&#x20; "action": "next\_song"

}

```



Validation :



```text

spotify existe

next\_song existe

```



↓



```text

Autorisé

```



\---



\# Exemple refusé



Le LLM produit :



```json

{

&#x20; "tool": "spotify",

&#x20; "action": "delete\_account"

}

```



Validation :



```text

delete\_account inexistant

```



↓



```text

Refus

```



\---



\# Interface ToolAction



Toutes les actions implémentées doivent suivre le même contrat.



```kotlin

interface ToolAction {



&#x20;   val tool: String



&#x20;   val action: String



&#x20;   fun execute(): ToolResult

}

```



\---



\# Objectifs



Cette interface garantit :



\- cohérence

\- extensibilité

\- validation uniforme

\- logging uniforme



\---



\# Exemple



```kotlin

class NextSongAction : ToolAction {



&#x20;   override val tool = "spotify"



&#x20;   override val action = "next\_song"



&#x20;   override fun execute(): ToolResult {



&#x20;       spotifyController.next()



&#x20;       return ToolResult.success()

&#x20;   }

}

```



\---



\# Validation MCP



Avant toute exécution :



```text

1\. Tool existe ?

2\. Action existe ?

3\. Handler existe ?

4\. Exécution

```



\---



\# Validation détaillée



\## Étape 1



Vérifier le Tool.



Exemple :



```json

{

&#x20; "tool": "spotify",

&#x20; "action": "next\_song"

}

```



↓



```text

spotify existe

```



\---



\## Étape 2



Vérifier l'action.



```text

next\_song existe

```



\---



\## Étape 3



Vérifier l'implémentation.



```text

NextSongAction existe

```



\---



\## Étape 4



Exécuter.



```text

execute()

```



\---



\# Cas de refus



\## Tool inconnu



```json

{

&#x20; "tool": "bank"

}

```



↓



```text

Tool inconnu

```



\---



\## Action inconnue



```json

{

&#x20; "tool": "spotify",

&#x20; "action": "delete\_song"

}

```



↓



```text

Action inconnue

```



\---



\## Handler absent



Registry :



```text

next\_song

```



Implémentation :



```text

Absente

```



↓



```text

Refus

```



\---



\# Objectif sécurité



Même si le modèle devient plus intelligent :



```text

Llama

Qwen

Phi

Gemma

```



La sécurité reste inchangée.



Le modèle ne possède aucune permission supplémentaire.



\---



\# Principe Zero Trust



Kiro ne fait jamais confiance au LLM.



Tout ce que le LLM produit est considéré comme :



```text

Non validé

```



jusqu'à validation par MCP.



\---



\# Ajout d'une nouvelle action



Pour ajouter une capacité :



\## 1



Ajouter l'action au Registry.



```kotlin

"open\_spotify"

```



\---



\## 2



Créer le Handler.



```kotlin

class OpenSpotifyAction

```



\---



\## 3



Enregistrer le Handler.



\---



\## 4



Ajouter les tests.



\---



\# Checklist



Une action est valide si :



\- \[ ] Tool enregistré

\- \[ ] Action enregistrée

\- \[ ] Handler implémenté

\- \[ ] Tests présents

\- \[ ] Logging actif



\---



\# Principe final



Le LLM propose.



Le MCP valide.



Le Tool exécute.



L'utilisateur conserve toujours le contrôle.

