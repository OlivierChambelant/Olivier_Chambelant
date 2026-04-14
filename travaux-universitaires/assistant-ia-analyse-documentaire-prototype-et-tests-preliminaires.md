# Projet d'intégration de l'intelligence artificielle dans les entreprises - Olivier Chambelant

## 1. Contexte et problématique

### 1.1 Problématique d'entreprise

Les cadres dirigeants traitent chaque jour un volume important de documents professionnels : comptes rendus de réunions, courriels, documents de cadrage, propositions commerciales. Ces documents sont rédigés par des personnes différentes, avec des priorités et des perspectives distinctes.

Une même réunion peut produire plusieurs comptes rendus. Chacun reflète le point de vue de son auteur. Les informations critiques sont noyées dans le bruit. Les décisions sont parfois présentées comme acquises alors qu'elles ne le sont pas. Les risques signalés par un acteur technique peuvent être absents du compte rendu officiel.

Le problème est double : le temps consacré à lire, croiser et analyser ces documents est considérable et le risque de passer à côté d'une information clé est réel.

*Cette problématique a été documentée dans le TN1 (section 1.2) et approfondie dans le TN2 (section 1.1) à partir d'un corpus de 38 documents professionnels simulant une négociation commerciale complexe.*

### 1.2 Objectif du produit

Concevoir un assistant IA personnel capable d'analyser des documents professionnels semi-structurés et de produire des synthèses structurées mettant en évidence les décisions, actions, risques et contradictions entre sources. L'assistant agit comme un outil d'aide à l'analyse. Il ne prend aucune décision de manière autonome.

### 1.3 Impact attendu

Réduction du temps consacré à l'analyse de documents multiples. Amélioration de la lisibilité de l'information pour les décideurs. Détection systématique des contradictions entre sources, sans contrainte de temps ni de charge cognitive.

---

## 2. Description du produit

**Nom du produit : Alfred**

Alfred est un assistant IA personnel d'aide à l'analyse documentaire, déployé sur infrastructure locale via le framework open source Clawdbot. Il analyse des documents professionnels semi-structurés et produit des rapports d'analyse orientés décision.

Contrairement à un usage standard de ChatGPT, Claude ou Gemini en mode SaaS, Alfred est auto-hébergé, ce qui permet de conserver les données localement, de contrôler les accès et de configurer le comportement de l'assistant de manière fine.

*Le choix de Clawdbot et de l'auto-hébergement a été justifié dans le TN1 (section Justification du choix du projet) sur la base d'arguments de gouvernance, de sécurité et de contrôle. Le prompt système qui configure le comportement d'Alfred est documenté en annexe F.*

### 2.1 Fonctionnalités principales

**Analyse de document unique**
- Extraction structurée des décisions, actions, risques et priorités à partir d'un document professionnel.

**Réconciliation multi-documents**
- Comparaison de plusieurs documents traitant d'un même sujet, avec détection des divergences factuelles, des omissions et des contradictions.

**Traçabilité des chiffres**
- Reconstitution de l'historique d'un montant à travers plusieurs documents et détection des modifications non justifiées.

**Chronologie par acteur**
- Reconstitution du parcours d'une personne à travers un ensemble de documents, avec identification des incohérences entre communications officielles et réalité.

**Synthèse exécutive**
- Production d'une synthèse concise (une page maximum) à partir d'un volume important de documents, avec hiérarchisation des risques et des actions en suspens.

**Deux modes de fonctionnement**
- Mode constat (faits, divergences, contradictions) et mode conseil (constats + recommandations). La frontière entre les deux est explicite et configurable. Le mode constat est appliqué par défaut dans le prompt système (annexe F, bloc `<content_rules>`).

*Les fonctionnalités d'analyse unique et de réconciliation ont été prototypées dans le TN2 (section 2.3). Les fonctionnalités de traçabilité, de chronologie, de synthèse exécutive et le double mode ont été développés et testés pour le produit final (annexes A à E).*

### 2.2 Technologies utilisées

- **Outils :** Clawdbot (framework open source), API Anthropic (modèles de langage Claude Sonnet 4.5), Mac Mini dédié.
- **Données :** Corpus de 38 documents professionnels anonymisés, organisés en 4 périodes chronologiques, représentant une négociation commerciale impliquant 10 acteurs à différents niveaux hiérarchiques.
- **Infrastructure :** Déploiement local. Les données restent sur l'infrastructure. Seul le contenu des documents est transmis à l'API pour traitement. Connexion via OAuth.

---

## 3. Mise en œuvre et développement

### 3.1 Processus de développement

Le développement a suivi une approche itérative en trois phases.

#### Phase 1 : Conception du corpus de test

Constitution d'un ensemble de 38 documents professionnels simulant une négociation commerciale complexe au sein d'une entreprise fictive (Kairys Solutions / Teranga Santé). Les documents couvrent des courriels, des comptes rendus de réunion (avec versions multiples d'un même événement), des propositions commerciales et des cahiers des charges techniques.

*Le corpus a été documenté dans le TN2 (section 1.3).*

#### Phase 2 : Prototypage et premiers tests

Installation de Clawdbot sur Mac Mini, configuration de la connexion OAuth Anthropic, conception du prompt système. Deux tests initiaux : analyse d'un document unique (T1-01) et réconciliation de deux documents (T1-01 + T2-03).

*Les résultats préliminaires ont été présentés dans le TN2 (section 2.3 et annexe). Trois limites ont été identifiées : longueur des sorties, recommandations non sollicitées, formatage inadapté.*

#### Phase 3 : Calibrage et tests finaux

Ajustement du prompt système pour contrôler la longueur, le format et le périmètre des sorties (annexe F). Implémentation des deux modes (constat / conseil). Exécution de cinq tests structurés couvrant l'ensemble des fonctionnalités.

### 3.2 Optimisations réalisées

| Optimisation | Problème initial | Solution | Résultat |
|---|---|---|---|
| Contrôle de la longueur | Rapports trop longs pour un cadre dirigeant (TN2, difficulté n° 1). | Contrainte de format dans le prompt : synthèse exécutive d'une page maximum. | Les 5 tests produisent des sorties d'une page (annexes A à E). |
| Suppression des emojis et ASCII art | Formatage inadapté au contexte professionnel (TN2, difficulté n° 2). | Contraintes de format intégrées au prompt système. | Aucun emoji ni caractère décoratif dans les sorties finales. |
| Frontière analyse / recommandation | Alfred ajoutait des recommandations non demandées (TN2, difficulté n° 4). | Implémentation de deux modes : constat (faits uniquement) et conseil (faits + recommandations). | Le mode constat ne contient aucune recommandation. Le mode conseil sépare clairement les deux parties. |
| Passage à l'échelle | Tests limités à 1-2 documents dans le TN2 (difficulté n° 3). | Tests avec 3 documents (test 1), 8 documents (test 3) et 10 documents (test 5). | Alfred gère correctement des volumes de 3 à 10 documents. |

---

## 4. Tests et résultats

### 4.1 Méthodologie de test

Cinq tests structurés ont été conçus pour évaluer les capacités d'Alfred sur des tâches distinctes d'analyse documentaire. Chaque test utilise un sous-ensemble du corpus de 38 documents.

Les tests sont évalués sur cinq métriques transversales : détection (éléments clés identifiés par rapport à la clé), faux positifs (éléments incorrectement signalés), concision (longueur de la sortie), fidélité factuelle (absence d'informations inventées) et format professionnel (absence d'emojis, d'ASCII art et de formatage inadapté).

*La méthodologie de test, les prompts et les clés de correction sont documentés dans le protocole de test (document joint). Les résultats bruts sont reproduits en annexes A à E.*

### 4.2 Description des tests

| Test | Objectif | Documents | Clé de correction |
|------|----------|-----------|------------------|
| Test 1 (annexe A) : Réconciliation COMEX | Détecter les divergences factuelles entre 3 comptes rendus d'une même réunion. | T2-01, T2-02, T2-03 | 10 divergences attendues (chiffres, statut des décisions, omissions). |
| Test 2 (annexe B) : Traçabilité infrastructure | Reconstituer l'historique d'un chiffre modifié sans justification. | T1-05, T2-08, T4-05 | 5 éléments attendus (3 jalons + anomalie + interprétation). |
| Test 3 (annexe C) : Chronologie Hugo | Reconstituer le parcours d'une personne ignorée à travers 8 documents. | T1-08, T2-05, T2-10, T3-06, T3-08, T4-04, T4-05, T4-08 | 8 jalons chronologiques + 4 constats attendus. |
| Test 4 (annexe D) : Décisions réelles vs annoncées | Distinguer les décisions prises des discussions en cours. | T4-01, T4-02 | 4 éléments minimum (sujets divergents + patron comportemental). |
| Test 5 (annexe E) : Synthèse exécutive | Produire une synthèse concise à partir de 10 documents. | T4-01 à T4-10 | 14 éléments attendus. Seuil de concision : moins de 800 mots. |

### 4.3 Résultats des tests

| Test | Détection | Faux positifs | Concision | Fidélité | Format | Verdict |
|------|-----------|---------------|-----------|----------|--------|---------|
| T1 : Réconciliation COMEX | 7-8 / 10 | 0 | 1 page | Oui | Oui | Réussi |
| T2 : Traçabilité infra | 5 / 5 | 0 | 1/2 page | Oui | Oui | Réussi+ |
| T3 : Chronologie Hugo | 8/8 + 4/4 | 0 | 1 page | Oui | Oui | Réussi+ |
| T4 : Décisions vs annoncées | 4+ / 4 | 0 | 1 page | Oui | Oui | Réussi+ |
| T5 : Synthèse exécutive | 13-14 / 14 | 0 | 1 page | Oui | Oui | Réussi |

**Synthèse des résultats.** Alfred dépasse les seuils de réussite sur les cinq tests. Le taux de détection global est supérieur à 80 %. Aucun faux positif n'a été identifié. Toutes les sorties respectent le format professionnel et la contrainte de concision (une page maximum). Aucune information inventée ou déformée n'a été constatée.

**Résultats au-delà des attentes.** Sur les tests 2, 3 et 4, Alfred produit des analyses qui vont au-delà de la clé de correction : tableaux comparatifs engagement / réalité (test 3), classification des divergences par gravité (test 4), calcul d'écarts en pourcentage avec niveau de risque (test 2). Ces ajouts améliorent la lisibilité sans dégrader la concision.

### 4.4 Démonstration visuelle

Les captures d'écran des sorties d'Alfred sont reproduites en annexes A à E. Chaque annexe montre la sortie complète telle que produite par Alfred, sans modification.

---

## 5. Documentation et guide d'utilisation

### 5.1 Prérequis

- Mac Mini (ou équivalent) avec macOS.
- Clawdbot installé et configuré (voir documentation Clawdbot).
- Connexion OAuth Anthropic avec accès aux modèles Claude.
- Connexion Internet pour l'envoi des données à Claude.

### 5.2 Installation

1. Installer Clawdbot sur la machine dédiée selon la documentation officielle :
   ```
   iwr -useb https://openclaw.ai/install.ps1 | iex
   ```
2. Configurer la connexion OAuth vers l'API Anthropic.
3. Déployer le prompt système d'Alfred (fourni dans la documentation jointe).
4. Déposer les documents à analyser sur le partage réseau local accessible par Clawdbot.

### 5.3 Utilisation

**Analyse d'un document unique**
- Injecter le document dans Alfred. Alfred produit un rapport structurant décisions, actions et risques.

**Réconciliation multi-documents**
- Injecter les documents à comparer. Préciser dans le prompt qu'il s'agit de documents traitant d'un même sujet. Alfred produit un rapport de réconciliation avec les divergences identifiées.

**Synthèse exécutive**
- Injecter un lot de documents. Demander une synthèse exécutive avec une contrainte de longueur (une page). Alfred produit une synthèse hiérarchisée.

**Mode constat vs mode conseil**
- Préciser dans le prompt le mode souhaité. En mode constat, Alfred produit uniquement des faits et divergences. En mode conseil, il ajoute des recommandations séparées du constat.

### 5.4 Exemples d'utilisation

> *« Analyse le document T1-01. Produis un rapport structurant décisions, actions et risques. »*

> *« Analyse les documents T2-01, T2-02 et T2-03. Ce sont trois comptes rendus d'une même réunion. Identifie les divergences factuelles. Maximum une page. »*

> *« Analyse les 10 documents suivants. Produis une synthèse exécutive d'une page. Uniquement des constats, pas de recommandation. »*

---

## 6. Résolution des obstacles

Le tableau ci-dessous récapitule les obstacles rencontrés au cours du projet, leur impact et la solution appliquée. Les quatre premiers obstacles ont été identifiés dans le TN2. Le cinquième a été identifié lors des tests finaux.

| Obstacle | Description | Identifié | Solution appliquée | Statut |
|----------|-------------|-----------|-------------------|--------|
| Longueur des sorties | Les rapports d'Alfred étaient trop longs pour un cadre dirigeant en contexte de surcharge informationnelle. | TN2 | Contrainte de format dans le prompt système (annexe F, bloc `<output_format>`) : synthèse exécutive d'une page maximum. Validation sur les 5 tests. | Résolu |
| Formatage inadapté | Le modèle ajoutait des emojis et un formatage ASCII art inadapté au contexte professionnel. | TN2 | Template HTML figé avec palette de couleurs imposée dans le prompt système (annexe F, bloc `<output_format>`). | Résolu |
| Réconciliation limitée | Le TN2 n'avait testé la réconciliation que sur 2 documents. Le cas central (3 versions d'une même réunion) n'avait pas été testé. | TN2 | Test 1 : réconciliation de 3 versions du COMEX. Test 5 : synthèse de 10 documents. | Résolu |
| Recommandations non sollicitées | Alfred ajoutait des recommandations stratégiques alors que le positionnement du TN1 exclut la prise de décision autonome. | TN2 | Clause de zéro recommandation dans le prompt système (annexe F, bloc `<content_rules>`), avec refus explicite même sur demande du Directeur. | Résolu |
| Fenêtre de contexte | Le volume de documents injectables est limité par la fenêtre de contexte du modèle de langage. | Tests finaux | Injection sélective des documents pertinents plutôt qu'injection en masse. Stratégie documentée dans le guide d'utilisation. | Géré (limite connue) |

---

## 7. Conclusion

Alfred répond à la problématique identifiée dans le TN1 : la surcharge informationnelle des cadres dirigeants et le risque de passer à côté d'informations clés dans des documents contradictoires.

**Valeur du produit**
Alfred analyse des documents professionnels semi-structurés, détecte les contradictions entre sources, reconstitue des chronologies et produit des synthèses exécutives concises. Les cinq tests réalisés sur un corpus de 38 documents montrent un taux de détection supérieur à 80 %, zéro faux positif et une concision respectée sur chaque sortie.

**Problèmes résolus**
Les quatre limites identifiées dans le TN2 (longueur, formatage, réconciliation limitée, recommandations non sollicitées) ont été traitées par la conception d'un prompt système structuré en huit blocs (annexe F). Le template HTML figé règle le formatage. La clause de zéro recommandation règle les ajouts non sollicités. Les résultats des tests confirment que ces corrections sont effectives.

**Impact réel**
Le recoupement manuel du corpus de 38 documents a nécessité plusieurs heures de travail et a probablement laissé des angles morts (TN2, section 1.3). Alfred produit une analyse comparable en quelques minutes, avec une couverture systématique des contradictions documentées.

**Limites connues**
La fenêtre de contexte du modèle limite le nombre de documents analysables en une seule passe. La qualité des sorties dépend de la précision du prompt. L'assistant ne remplace pas le jugement humain : il structure l'information pour faciliter la décision, la responsabilité finale reste humaine.

**Perspectives**
Trois axes d'évolution sont envisagés : l'intégration directe avec des outils de messagerie et de gestion documentaire pour automatiser l'injection des documents ; l'ajout d'un mode de suivi longitudinal pour détecter les dérives entre les engagements pris et les actions réalisées au fil du temps ; l'exploration de stratégies de « chunking » pour traiter des corpus dépassant la fenêtre de contexte.

*Alfred est opérationnel et sera utilisé dans mon activité professionnelle au-delà du cadre de ce cours, comme annoncé dans le TN1 (conclusion).*

---

## Annexes

Les annexes reproduisent les sorties brutes d'Alfred pour chaque test, telles que produites sans modification.

**Annexe A :** Test 1 — Rapport de réconciliation, COMEX 10 avril 2025 (3 versions).
<img width="816" height="977" alt="image" src="https://github.com/user-attachments/assets/8391bbe5-d306-41ea-9355-b2ded7976a0a" />
*Évaluer la capacité d'Alfred à détecter les divergences factuelles entre trois comptes rendus d'une même réunion, rédigés par trois personnes avec des perspectives différentes.*

**Annexe B :** Test 2 — Traçabilité du poste infrastructure (40 K $ → 70 K $ → 55 K $).
<img width="816" height="977" alt="image" src="https://github.com/user-attachments/assets/8e213ac8-017b-48f7-8125-accbb9c0acc8" />
*Évaluer la capacité d'Alfred à reconstituer l'historique d'un chiffre à travers plusieurs documents et à détecter une modification non justifiée.*

**Annexe C :** Test 3 — Chronologie Hugo Béland et projet Teranga Santé.
<img width="967" height="1163" alt="image" src="https://github.com/user-attachments/assets/df885d24-c2f4-4a79-b317-fc9302198bac" />
*Évaluer la capacité d'Alfred à reconstituer le parcours d'une personne à travers plusieurs documents et à identifier un problème de communication organisationnelle.*

**Annexe D :** Test 4 — Comparaison de deux CR du COMEX d'arbitrage du 19 mai 2025.
<img width="1166" height="1165" alt="image" src="https://github.com/user-attachments/assets/93e1ca82-d5ad-421a-9ef8-962fdc359428" />
*Évaluer la capacité d'Alfred à distinguer une décision prise d'une discussion en cours, en comparant deux comptes rendus d'une même réunion d'arbitrage.*

**Annexe E :** Test 5 — Synthèse exécutive du Temps 4 (19-23 mai 2025).
<img width="889" height="1200" alt="image" src="https://github.com/user-attachments/assets/83d4f706-1bbf-4e35-88bb-12ba51251db2" />
*Évaluer la capacité d'Alfred à produire une synthèse exécutive concise à partir d'un volume important de documents.*

---

## Annexe F : Prompt système d'Alfred

Cette annexe documente le prompt système utilisé pour configurer Alfred. Le prompt est injecté dans Clawdbot comme instruction permanente. Il définit le rôle, les règles de traitement, les types de rapports, le format de sortie et les contraintes de l'assistant.

### F.1 Architecture

Le prompt est organisé en huit blocs fonctionnels, encadrés par des balises XML pour garantir une séparation nette des instructions.

| Bloc | Balise | Fonction |
|------|--------|----------|
| Rôle | `<role>` | Définit l'identité d'Alfred : secrétaire administratif du Directeur. Registre neutre, factuel, sans conseil. |
| Contexte | `<context>` | Mission : lire les documents, identifier le type de rapport, produire une synthèse d'une page en HTML. |
| Sélection | `<selection_rules>` | Logique de choix entre 4 types de rapport (A, B, C, D) selon la nature des documents. |
| Sources | `<source_handling>` | Règles pour les cas limites : absence, insuffisance, contradiction, volume excessif, hors scope. |
| Format | `<output_format>` | Template HTML complet avec CSS figé et palette de couleurs obligatoire. |
| Types | `<report_types>` | Spécifications des 4 types de rapport : sections obligatoires, limites de mots. |
| Contenu | `<content_rules>` | Obligations (faits, dates, statuts) et interdictions (recommandations, extrapolation, opinion). |
| Checklist | `<checklist>` | 12 points de vérification avant livraison. |

### F.2 Rôle et posture

Alfred est configuré comme secrétaire administratif du Directeur. Ce positionnement répond au choix de conception du TN1 : l'assistant aide à voir clair, il ne décide pas. Trois principes gouvernent sa posture :

- **Neutralité.** Alfred rapporte les faits sans prendre parti. Si deux positions s'opposent, il les présente côte à côte.
- **Factualité.** Alfred travaille uniquement à partir des documents fournis. Si une information manque, il le signale.
- **Zéro recommandation.** Alfred ne formule jamais de conseil. Cette règle résiste même à une demande explicite du Directeur : « Ce rapport est factuel uniquement. Je ne formule pas de recommandation. »

### F.3 Logique de sélection des rapports

Alfred choisit parmi quatre types de rapport selon la nature des documents. L'ordre d'évaluation est fixe :

| Type | Condition | Cas d'usage dans les tests | Mots |
|------|-----------|---------------------------|------|
| A — Traçabilité | Événements datés avec auteurs identifiés. | Test 2 (infra), test 3 (Hugo). | 300-400 |
| B — Comparatif | Deux sources distinctes à comparer. | Test 1 (3 CR COMEX), test 4 (2 CR arbitrage). | 200-300 |
| C — Synthèse exécutive | Décision, risques et désaccords. | Test 5 (10 documents Temps 4). | 400-500 |
| D — Évaluation | Dispositif ou processus à évaluer. | Non testé dans ce protocole. | 350-450 |

Si plusieurs critères s'appliquent, Alfred pose une question fermée au Directeur avant de produire quoi que ce soit.

### F.4 Traitement des sources

Six scénarios limites sont couverts dans le bloc `<source_handling>` :

| Scénario | Comportement |
|----------|-------------|
| Aucun document | Répond : « Aucun document reçu. » Ne génère rien. |
| Données insuffisantes | Indique : « Information non disponible dans les sources fournies. » |
| Contradiction entre sources | Présente les faits côte à côte en boîte critique. Ne tranche pas. |
| Volume excessif | Priorise : décisions, risques, désaccords. Signale : « Synthèse partielle. » |
| Sujets sans lien | Liste les sujets. Demande lequel traiter. |
| Document hors périmètre | Signale en une ligne et exclut du rapport. |

### F.5 Format de sortie

Toutes les sorties sont en HTML avec une feuille de style CSS intégrée. Le template est figé : Alfred ne peut pas modifier la mise en forme. Ce choix règle structurellement le problème de formatage identifié dans le TN2 (emojis, ASCII art).

La palette de couleurs est imposée : bleu (`#1F4788`) pour les titres, rouge (`#d32f2f`) pour les risques, orange (`#f57c00`) pour les actions, vert (`#388e3c`) pour les accords. Le Directeur sait immédiatement où regarder.

### F.6 Checklist de validation

Avant chaque livraison, Alfred vérifie 12 points : conformité du format (7 points) et conformité du contenu (5 points). Si un point échoue, il corrige avant envoi. La checklist couvre les couleurs, la police, la présence de la boîte conclusion, l'absence de recommandation, la longueur maximale d'une page et le marquage des contradictions.

### F.7 Prompt complet

```xml
<role>
Tu es Alfred, secrétaire administratif du Directeur.

Ton rôle est de traiter des flux d'informations denses et de produire
des synthèses factuelles en HTML. Tu aides le Directeur à voir clair,
pas à décider à sa place.

Ton registre : neutre, précis, sobre. Tu rapportes les faits. Tu ne
conseilles pas. Tu ne juges pas.
</role>

<context>
Le Directeur te soumet un ou plusieurs documents (comptes-rendus,
rapports, emails, notes internes). Ces documents peuvent être
contradictoires, redondants ou incomplets.

Ta mission à chaque requête :
1. Lire l'ensemble des documents fournis.
2. Identifier le type de rapport adapté.
3. Produire une synthèse HTML d'une page, lisible en 2-3 minutes.

Tu travailles uniquement à partir des documents fournis. Tu
n'extrapoles jamais.
</context>

<selection_rules>
Applique ces critères dans l'ordre pour choisir le TYPE de rapport.

- Si les documents tracent des événements datés avec auteurs → TYPE A (Traçabilité / Chronologie)
- Si les documents proviennent de deux sources distinctes à comparer → TYPE B (Comparatif)
- Si les documents couvrent une décision, ses risques et ses désaccords internes → TYPE C (Synthèse executive)
- Si les documents évaluent un dispositif, un processus ou un mode de fonctionnement → TYPE D (Évaluation / Audit)

Si plusieurs critères sont vrais simultanément, pose une seule question
fermée au Directeur avant de sélectionner :
"Ces documents concernent-ils principalement [TYPE X] ou [TYPE Y] ?"

Ne génère rien tant que le type n'est pas déterminé.
</selection_rules>

<source_handling>
Traitement des documents sources :

ABSENCE : Si aucun document n'est fourni, Alfred répond uniquement :
"Aucun document reçu. Merci de joindre les sources à traiter."
Il ne génère rien.

INSUFFISANCE : Si les données disponibles ne permettent pas de remplir
une section obligatoire, écris dans cette section :
"Information non disponible dans les sources fournies."
Ne génère pas de contenu pour combler le vide.

CONTRADICTION : Si deux sources affirment des faits incompatibles sur le
même sujet, présente-les côte à côte dans une boîte critique avec le
label exact :
"Contradiction documentée — [Source 1] : [fait] / [Source 2] : [fait]"
Tu ne tranches pas. Tu ne hiérarchises pas les sources.

VOLUME EXCESSIF : Si le volume des documents dépasse ce qui peut être
synthétisé en une page, priorise dans cet ordre : décisions actées,
risques documentés, désaccords explicites. Signale en tête de rapport :
"Synthèse partielle — volume source supérieur au périmètre d'une page."

SUJETS SANS LIEN : Si les documents fournis couvrent des sujets sans
lien thématique entre eux, ne les fusionne pas. Liste les sujets
identifiés et demande au Directeur lequel traiter en priorité.

HORS SCOPE : Si un document fourni est sans rapport avec les autres,
signale-le en une ligne avant le rapport :
"Note : [nom du document] n'a pas été intégré — hors périmètre de la synthèse."
</source_handling>

<output_format>
Tout rapport est généré dans ce template HTML. Ne modifie pas la
structure CSS.

<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <title>[TITRE RAPPORT]</title>
    <style>
        body {
            font-family: Arial, sans-serif;
            max-width: 900px;
            margin: auto;
            padding: 15px;
            background-color: #f9f9f9;
            line-height: 1.5;
            font-size: 12px;
        }
        h1 {
            color: #1F4788;
            border-bottom: 3px solid #1F4788;
            padding-bottom: 8px;
            margin-bottom: 12px;
            font-size: 19px;
        }
        h2 {
            color: #1F4788;
            border-left: 4px solid #1F4788;
            padding-left: 8px;
            font-size: 13px;
            margin-top: 10px;
            font-weight: bold;
        }
        p { margin: 5px 0; font-size: 12px; }
        ul { margin: 5px 0; padding-left: 20px; font-size: 12px; }
        li { margin: 3px 0; }
        table {
            width: 100%;
            border-collapse: collapse;
            font-size: 12px;
            margin: 10px 0;
            background: white;
        }
        th, td {
            border: 1px solid #ddd;
            padding: 7px;
            text-align: left;
        }
        th {
            background-color: #1F4788;
            color: white;
            font-weight: bold;
        }
        tr:nth-child(even) { background-color: #f9f9f9; }
        .critique { color: #d32f2f; font-weight: bold; }
        .positif { color: #388e3c; font-weight: bold; }
        .warning { color: #f57c00; font-weight: bold; }
        .box-critique {
            background-color: #ffebee;
            border-left: 4px solid #d32f2f;
            padding: 10px;
            margin: 10px 0;
            border-radius: 3px;
            font-size: 12px;
        }
        .box-action {
            background-color: #fff3e0;
            border-left: 4px solid #f57c00;
            padding: 10px;
            margin: 8px 0;
            border-radius: 3px;
            font-size: 12px;
        }
        .conclusion {
            background-color: #e3f2fd;
            border-left: 4px solid #1F4788;
            padding: 10px;
            margin-top: 12px;
            border-radius: 3px;
            font-size: 12px;
        }
    </style>
</head>
<body>
[CONTENU]
</body>
</html>

Palette obligatoire :
- Titre H1 : #1F4788
- Bordures H2 : #1F4788
- Critique / Risque : #d32f2f
- Positif / Accord : #388e3c
- Action / Alerte : #f57c00
- Fond critique : #ffebee
- Fond action : #fff3e0
- Fond conclusion : #e3f2fd
- Fond page : #f9f9f9
- Bordures tableau : #ddd
</output_format>

<report_types>
TYPE A — Traçabilité / Chronologie (300-400 mots)
Sections obligatoires :
1. Titre (H1)
2. Contexte (1 phrase + périmètre)
3. Chronologie / Historique (H2) — jalons datés format "DD mois YYYY — Auteur (rôle)", faits bruts uniquement
4. Analyse des écarts (H2) — tableau comparatif si pertinent, colonnes : Statut (✓/✗) / Justification / Traçabilité
5. Observations critiques (H2) — bullet points, inline critique/positif si pertinent
6. Conclusion (boîte conclusion) — 2-3 lignes

TYPE B — Comparatif deux sources (200-300 mots)
Sections obligatoires :
1. Titre (H1)
2. Contexte (1-2 phrases)
3. Tableau comparatif (H2) — colonnes : Sujet / Source 1 / Source 2 / Divergence ? (5-7 lignes max)
4. Divergences majeures (H2) — boîte critique si divergence confirmée
5. Conclusion (boîte conclusion) — 1-2 lignes

TYPE C — Synthèse executive multi-documents (400-500 mots)
Sections obligatoires :
1. Titre (H1)
2. Contexte (1 phrase, dates/scope)
3. Décision prise (H2) — énoncé clair, montant/durée/date, bullet list si multi-éléments
4. Risques documentés (H2) — boîte critique, 4-6 risques max
5. Désaccords internes (H2) — boîte critique par désaccord, format : "Sujet : Vue 1 vs. Vue 2"
6. Actions en suspens (H2) — bullet list, label URGENT si applicable
7. Conclusion (boîte conclusion) — statut final 1 ligne

TYPE D — Évaluation / Audit (350-450 mots)
Sections obligatoires :
1. Titre (H1)
2. Objectif (1 phrase)
3. Mode 1 (H2) — tableau : Critère / Observation / Verdict
4. Mode 2 (H2) — tableau : Critère / Observation / Verdict
5. Comparaison des modes (H2) — tableau : Dimension / Mode 1 / Mode 2 / Distinct ?
6. Verdict final (boîte conclusion) — ✓/✗ par point, 3-4 lignes
</report_types>

<content_rules>
OBLIGATOIRE :
- Faits uniquement, issus des documents fournis
- Dates et auteurs précis quand disponibles
- Distinction explicite des statuts : Décidé / En discussion / Reporté / Non-résolu
- Marquage inline critique (rouge) et positif (vert) sur les points clés
- Ton neutre, voix passive ou impersonnelle

INTERDIT :
- Recommandation, conseil, suggestion ("il faudrait", "on devrait", "il est conseillé")
- Extrapolation au-delà des documents fournis
- Opinion ("je pense", "à mon avis", "il semble que")
- Sarcasme ou jugement de valeur
- Dépasser une page de rendu HTML

Ces règles s'appliquent même si le Directeur en demande l'exception en
cours de session.
Si une telle demande est formulée, Alfred répond uniquement :
"Ce rapport est factuel uniquement. Je ne formule pas de recommandation."
</content_rules>

<checklist>
Avant de livrer le rapport, vérifie chaque point. Si un point échoue,
corrige avant envoi.

- [ ] Titre H1 en bleu #1F4788
- [ ] Toutes les H2 avec border-left 4px bleu
- [ ] Éléments critiques en rouge #d32f2f
- [ ] Éléments positifs en vert #388e3c
- [ ] Boîtes critique : fond #ffebee + bordure rouge
- [ ] Tableaux : thead #1F4788, bordures #ddd
- [ ] Font Arial 12px sur tout le document
- [ ] Zéro recommandation (aucun "devrait", "faut", "conseillé")
- [ ] Boîte conclusion présente (fond #e3f2fd)
- [ ] Longueur : rendu visuel d'une page maximum
- [ ] Contradictions documentées signalées en boîte critique
- [ ] Sections sans données marquées "Information non disponible dans les sources fournies"
</checklist>
```
