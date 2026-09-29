# Architecture des processus basés sur `cmpActionButton`

## Objectif

Dans le cas des processus lancés depuis l'écran principal, le mécanisme ne repose pas sur `StatutConfigTable.Actions`.

Le point d'entrée du comportement est le composant `cmpActionButton`.

Le composant déclare :

- l'action exécutée ;
- les statuts source ;
- le statut cible ;
- le contexte de filtrage ;
- le processus courant.

Les contrôles de l'interface réagissent ensuite aux variables et informations produites par ce composant.

# Principe général

Un processus est défini par son bouton.

```PlainText
cmpActionButton
        │
        ▼
Action
        │
        ▼
SourceStatuses
        │
        ▼
Initialisation du contexte
        │
        ▼
Filtrage
        │
        ▼
Sélection
        │
        ▼
Traitement
        │
        ▼
NextStatus
```

Le composant constitue le contrôleur principal du processus.

# Les trois propriétés fondamentales

## Action

Identifie le processus.

```PlainText
cmpActBtnDACValider.Action =
    _ActionOnList.DACValider
```

Cette propriété est utilisée pour :

- identifier le processus courant ;
- initialiser `vcCurrentProcess` ;
- piloter la visibilité du panneau de processus ;
- piloter certains mécanismes de filtrage ;
- fournir le libellé métier du bouton.

## SourceStatuses

Déclare les statuts autorisant l'exécution du processus.

```PlainText
cmpActBtnDACValider.SourceStatuses =
[
    _StatutDetail.DACValidation
]
```

Cette propriété est utilisée pour :

- déterminer les demandes éligibles ;
- calculer le nombre d'éléments traitables ;
- alimenter le filtrage lors du lancement du processus ;
- préparer la sélection des demandes concernées.

Une demande est traitable uniquement si son statut appartient à `SourceStatuses`.

## NextStatus

Déclare le statut cible principal du processus.

```PlainText
cmpActBtnDACValider.NextStatus =
    _StatutDetail.DACValidee
```

Cette propriété est utilisée lors de l'exécution du traitement.

Elle centralise :

- la transition métier attendue ;
- l'adaptation du filtre après traitement ;
- le maintien éventuel des demandes dans la liste après changement de statut.

# Détermination de la disponibilité d'un processus

##  Principe

 La disponibilité d'un processus est déterminée par deux mécanismes complémentaires : 

- le rôle de l'utilisateur ; 
- le contrôleur du processus (`cmpActionButton`). 

 Un utilisateur ne peut lancer un processus que si l'action correspondante est autorisée pour son rôle.

```PlainText
Action
      │
      ▼
Rôle utilisateur
      │
      ▼
Bouton visible
```

##  Contrôle par le rôle

 La visibilité standard du bouton repose sur :

```PlainText
With(
    {MyAction : Self.Action};
    MyAction in vvUserRole.Actions
)
```

Le composant vérifie si l'action associée au bouton est présente dans la collection des actions autorisées pour le rôle courant.

 Cette approche permet : 

- de centraliser les autorisations dans le catalogue des rôles ; 
- d'éviter de dupliquer les règles d'accès dans chaque bouton ; 
- de réutiliser le même composant pour plusieurs processus.

# Variables de contexte produites par le processus

Lors du lancement du processus, le bouton initialise principalement :

```PlainText
vcCurrentProcess
vcCurrentProcessStep
vcApplyFilter
vcApplyFilterOnStatus
vcCurrentProcessCompleted
```

Ces variables sont ensuite utilisées par :

- les filtres de demandes ;
- les panneaux de processus ;
- la gestion de la sélection ;
- certains comportements d'interface.

Le moteur de processus fonctionne donc principalement à partir du contexte produit par le bouton.

# Exemple complet

```PlainText
cmpActBtnDACValider.Action =
    _ActionOnList.DACValider

cmpActBtnDACValider.SourceStatuses =
[
    _StatutDetail.DACValidation
]

cmpActBtnDACValider.NextStatus =
    _StatutDetail.DACValidee
```

Lecture fonctionnelle :

```PlainText
Action :
    DACValider

Demandes concernées :
    DACValidation

Résultat attendu :
    DACValidee
```

L'ensemble du comportement du processus peut être compris à partir de ces trois propriétés.

# Rôle résiduel de StatutConfigTable.Actions

`StatutConfigTable.Actions` reste utilisé pour les actions pilotées par le statut courant, principalement dans l'écran de détail.

Exemples :

```PlainText
_ActionOnForm.AddDoc
_ActionOnForm.PromAppro
_ActionOnForm.PromRefus
``
```

Le mécanisme suit alors le schéma :

```PlainText
StatutConfigTable.Actions
        │
        ▼
vcStatutConfig.Actions
        │
        ▼
Écran de détail
```

Ces actions sont consommées directement par le formulaire de la demande courante.
