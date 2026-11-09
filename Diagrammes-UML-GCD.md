# Diagrammes UML GCD

**Légende** — Sur GCD : en ligne. Hors GCD : démarche externe. Proposition : amélioration envisagée. À confirmer : règle à vérifier.

## 1. Activité — inscription initiale (parcours proposé)

La consultation sans compte, le panier de trois vœux et le redépôt après correction font partie du parcours proposé.

```mermaid
flowchart TD
  A([Début]) --> B["Valider et publier les sujets"]
  B --> C["Consulter les sujets sans compte et choisir 1 à 3 vœux"]
  C --> D["Se connecter ou créer et activer un compte"]
  D --> E["Soumettre la candidature et les pièces sur GCD"]
  E --> F{"Dossier en ligne complet ?"}
  F -->|Non — proposition| G["Corriger les pièces signalées"]
  G --> E
  F -->|Oui| H["Présélectionner, mener les entretiens et établir le PV"]
  H --> I["Publier les résultats"]
  I --> J{"Résultat du vœu ?"}
  J -->|Liste d’attente| K["Attendre la mise à jour"]
  K --> I
  J -->|Non retenu| L([Fin pour ce vœu])
  J -->|Liste principale| M["Préparer et déposer le dossier papier hors GCD"]
  M --> N{"Dossier papier conforme ?"}
  N -->|Non| O["Corriger le dossier papier"]
  O --> M
  N -->|Oui| P["Valider l’inscription et remettre le reçu"]
  P --> Q([Inscription terminée])
```

## 2. Activité — réinscription annuelle

Les conditions d’année et le formulaire final viennent du guide GCD 2026–2027. La boucle de correction des pièces est une proposition. Les suites d’un refus et les modalités de recours restent à confirmer.

```mermaid
flowchart TD
  A([Début]) --> B["Doctorant : saisir le rapport d’activités"]
  B --> C{"Dérogation demandée en 4e, 5e ou 6e année ?"}
  C -->|Oui| D["Doctorant : justifier la demande"]
  C -->|Non| E["Doctorant : préparer et faire signer la fiche 1/2"]
  D --> E
  E --> F{"Première inscription en 2023–2024 ou après ?"}
  F -->|Oui| G["Directeur de thèse : désigner le comité et saisir son avis motivé"]
  G --> H["Comité : signer l’avis à trois membres"]
  H --> I["Doctorant : charger sur GCD les pièces signées et l’avis si requis"]
  F -->|Non| I
  I --> J{"Directeur de thèse : pièces complètes ?"}
  J -->|Non| K["Doctorant : compléter les pièces signalées"]
  K --> I
  J -->|Oui| L{"Directeur de thèse : dossier approuvé ?"}
  L -->|Oui| M{"Directeur de structure : dossier approuvé ?"}
  M -->|Oui| N{"Directeur du CED : dossier confirmé ?"}
  L -->|Non| X["Consulter la décision et les suites applicables"]
  M -->|Non| X
  N -->|Non| X
  N -->|Oui| O["Doctorant : télécharger la fiche 2/2 signée par le CED et l’imprimer en couleur"]
  O --> P["Doctorant : déposer le dossier papier auprès du service compétent, hors GCD"]
  P --> Q([Dossier papier déposé])
```

## 3. États — dossier de réinscription

Le suivi distingue la validation sur GCD du dépôt administratif papier. Le retour « pièces à compléter » est proposé. Les motifs et les suites des décisions défavorables restent à confirmer.

```mermaid
stateDiagram-v2
  state "Brouillon" as Brouillon
  state "Avis du comité attendu" as Avis
  state "Pièces déposées" as Depose
  state "Pièces à compléter" as ACompleter
  state "Validation du directeur de thèse en attente" as These
  state "Validation de la structure en attente" as Structure
  state "Confirmation du CED en attente" as CED
  state "Validé sur GCD" as Valide
  state "Dossier papier déposé" as Papier
  state "Décision défavorable" as Refus
  [*] --> Brouillon
  Brouillon --> Avis : avis requis
  Avis --> Depose : avis signé et pièces chargés
  Brouillon --> Depose : pièces chargées, avis non requis
  Depose --> ACompleter : pièces incomplètes
  ACompleter --> Depose : pièces corrigées et redéposées
  Depose --> These : pièces complètes
  These --> Structure : approbation du directeur de thèse
  Structure --> CED : approbation du directeur de structure
  CED --> Valide : confirmation du CED
  These --> Refus : décision défavorable
  Structure --> Refus : décision défavorable
  CED --> Refus : décision défavorable
  Valide --> Papier : remise du dossier papier
  Papier --> [*]
```

## 4. Classes — candidature avec panier de vœux

Une candidature peut rester sans vœu au brouillon. À la soumission, elle doit contenir de un à trois vœux portant sur des sujets distincts. Les pièces sont rattachées à la candidature et servent à tous ses vœux. Le résultat est suivi par vœu.

```mermaid
classDiagram
  class Candidat {
    +String identifiant
    +String nom
    +String email
  }
  class Candidature {
    +String identifiant
    +Date dateDepot
    +String statutDossier
  }
  class Voeu {
    +String identifiant
    +String resultatSelection
  }
  class SujetDeThese {
    +String identifiant
    +String titre
    +String statutPublication
  }
  class Document {
    +String identifiant
    +String typeDocument
    +String nomFichier
    +Date dateDepot
  }
  Candidat "1" -- "0..*" Candidature : dépose
  Candidature "1" *-- "0..3" Voeu : contient
  Voeu "0..*" --> "1" SujetDeThese : concerne
  Candidature "1" *-- "0..*" Document : contient
```

## Sources de travail

- [Diagrammes et synthèses du dépôt devweb](https://github.com/youssefelhathat-cmd/devweb)
- [Diagrammes de référence d’omardarif](https://github.com/omardarif/UML-diagrame)
- [Diagramme d’états de Zineb Elkrichi](https://github.com/ZinebElkrichi/TP-DEV-LOGICIELS/blob/main/diagramme_etats_transmission_candidature.png), référence pour le retour du dossier à compléter.
- Guide GCD de réinscription 2026–2027, sections sur la dérogation, l’avis du comité de thèse et le dépôt du dossier administratif.

