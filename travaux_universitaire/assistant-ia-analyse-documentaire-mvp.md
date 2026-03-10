# Projet d'intégration de l'intelligence artificielle dans les entreprises
Olivier Chambelant

## 1. Contexte et problématique

### 1.1 Problématique d'entreprise

Les cadres dirigeants traitent chaque jour un volume important de documents professionnels : comptes rendus de réunions, courriels, documents de cadrage, propositions commerciales. Ces documents sont rédigés par des personnes différentes, avec des priorités et des perspectives distinctes.

Une même réunion peut produire plusieurs comptes rendus. Chacun reflète le point de vue de son auteur. Les informations critiques sont noyées dans le bruit. Les décisions sont parfois présentées comme acquises alors qu'elles ne le sont pas. Les risques signalés par un acteur technique peuvent être absents du compte rendu officiel.

Le problème est double. Le temps consacré à lire, croiser et analyser ces documents est considérable. Et le risque de passer à côté d'une information clé est réel. Dans un contexte de direction, une contradiction non détectée entre deux sources peut mener à une décision fondée sur des informations incomplètes ou inexactes.

### 1.2 Objectifs du projet

L'objectif du projet est de concevoir un assistant IA personnel capable d'analyser des documents professionnels semi-structurés et de produire des synthèses structurées mettant en évidence les décisions, actions, risques et contradictions entre sources. L'assistant agit comme un outil d'aide à l'analyse. Il ne prend aucune décision de manière autonome.

Trois objectifs opérationnels ont guidé le projet :

- Réduire le temps consacré à l'analyse de documents multiples.
- Améliorer la lisibilité de l'information pour les décideurs.
- Détecter systématiquement les contradictions entre sources, sans contrainte de temps ni de charge cognitive.

### 1.3 Contexte

Ce projet s'inscrit dans mon activité professionnelle. En tant que directeur des systèmes d'information dans une entreprise éditrice de logiciels SIRH, je suis confronté quotidiennement à cette surcharge informationnelle. Je traite des comptes rendus de réunions de pilotage, des courriels de directions métier, des propositions commerciales et des documents de cadrage technique. Ces documents proviennent d'acteurs aux intérêts divergents et reflètent des positions parfois contradictoires.

Pour évaluer la solution, j'ai constitué un corpus de test de 38 documents professionnels anonymisés, organisés en quatre périodes chronologiques, représentant une négociation commerciale impliquant une dizaine d'acteurs à différents niveaux hiérarchiques. Ce corpus simule les conditions réelles d'une situation de direction : informations contradictoires, omissions, arrondis dans le mauvais sens, décisions présentées comme acquises alors qu'elles ne le sont pas.

### 1.4 Importance et impact attendu

L'enjeu n'est pas d'automatiser la prise de décision. L'enjeu est de fournir un outil d'aide à l'analyse qui structure l'information, identifie les contradictions entre sources et met en évidence les éléments qui requièrent l'attention du décideur. La responsabilité de la décision reste humaine.

L'impact attendu est triple : un gain de temps sur l'analyse documentaire, une couverture plus systématique des contradictions entre sources, et une meilleure lisibilité de l'information pour la prise de décision.

---

## 2. Méthodologie

Le projet a suivi une approche itérative en trois phases : constitution du corpus de test, prototypage et premiers tests, puis calibrage et tests finaux.

### 2.1 Analyse des besoins

Le point de départ est un constat professionnel. Le recoupement manuel de documents contradictoires prend plusieurs heures, reste incomplet et ne garantit pas que toutes les incohérences soient détectées. Le besoin est un outil capable de croiser systématiquement les sources, d'identifier les divergences factuelles et de produire une synthèse lisible en quelques minutes.

Pour évaluer cette capacité, j'ai constitué un corpus de 38 documents professionnels anonymisés. Le corpus simule une négociation commerciale complexe entre une entreprise éditrice de logiciels (Kairys Solutions) et un client du secteur santé (Groupe Teranga Santé). Les documents couvrent quatre périodes chronologiques :

- **Premier contact et qualification :** courriels d'estimation, expression de besoin client, échanges internes.
- **Cadrage interne :** trois versions d'un même comité de direction, alertes techniques, échanges RH.
- **Réunion client et négociation :** compte rendu officiel et version interne, révision budgétaire, réponses des directions.
- **Arbitrage et proposition :** proposition commerciale, cahier des charges technique, réactions internes.

Le corpus implique une dizaine d'acteurs à différents niveaux hiérarchiques : un dirigeant qui pousse pour signer, une direction technique qui protège la qualité du produit, une direction RH qui défend les conditions de ses équipes, une direction des systèmes d'information prise en étau entre les trois. Les documents contiennent des ambiguïtés volontaires : chiffres arrondis dans le mauvais sens, réserves omises dans les comptes rendus officiels, communications formelles contredites par des échanges internes.

**Sources de données :** documents professionnels anonymisés, organisés en fichiers individuels par période chronologique.
**Outils utilisés :** Clawdbot (framework open source), API Anthropic (Claude Sonnet 4.5), Mac Mini dédié sous macOS.

### 2.2 Conception et implémentation

La solution repose sur Clawdbot, un framework open source d'assistant IA persistant, déployé sur un Mac Mini dédié. J'ai configuré une instance nommée Alfred, connectée à l'API Anthropic (modèle Claude Sonnet 4.5) via OAuth.

L'architecture comprend trois composants :

- **Entrée :** documents professionnels en format texte, injectés individuellement ou par lot.
- **Traitement :** Clawdbot orchestre les appels à l'API Anthropic.
- **Sortie :** rapports d'analyse produits en HTML avec une feuille de style figée.

Le choix de l'auto-hébergement répond à trois exigences : conserver les données localement, contrôler les accès et configurer le comportement de l'assistant de manière fine. Seul le contenu des documents est transmis à l'API pour traitement.

Le produit technique central n'est pas du code. C'est le prompt système. Il est structuré en huit blocs XML : rôle, contexte, logique de sélection, traitement des sources, format de sortie, types de rapport, règles de contenu, checklist de validation.

Chaque bloc répond à un problème précis :

- **Rôle (`<role>`) :** Alfred est configuré comme secrétaire administratif du Directeur. Il rapporte les faits. Il ne conseille pas. Il ne juge pas.
- **Contenu (`<content_rules>`) :** interdiction de formuler des recommandations, des opinions ou des extrapolations. Cette règle résiste même à une demande explicite du Directeur.
- **Format (`<output_format>`) :** template HTML figé avec palette de couleurs imposée. Le Directeur sait immédiatement où regarder : bleu pour les titres, rouge pour les risques, orange pour les actions, vert pour les accords.
- **Sélection (`<selection_rules>`) :** quatre types de rapport (traçabilité, comparatif, synthèse exécutive, évaluation) sélectionnés automatiquement selon la nature des documents.

L'extrait suivant montre le bloc `<content_rules>`, qui définit les règles de contenu appliquées à chaque rapport :

```xml
<content_rules>
OBLIGATOIRE :
- Faits uniquement, issus des documents fournis
- Dates et auteurs précis quand disponibles
- Distinction explicite : Décidé / En discussion / Reporté
- Ton neutre, voix passive ou impersonnelle
INTERDIT :
- Recommandation, conseil, suggestion
- Extrapolation au-delà des documents fournis
- Opinion, sarcasme ou jugement de valeur
- Dépasser une page de rendu HTML
Ces règles s'appliquent même si le Directeur en demande l'exception.
</content_rules>
```

Ce bloc illustre le positionnement central du projet : Alfred aide à voir clair, il ne décide pas. L'interdiction de recommandation est structurelle, pas optionnelle. Le prompt complet (huit blocs) est documenté en annexe du TN3.

### 2.3 Tests et optimisation

Le développement a traversé deux cycles de test.

**Cycle 1 (TN2) :** deux tests préliminaires — analyse d'un document unique (courriel de direction) et réconciliation de deux documents (courriel + notes de comité de direction). Ces tests ont révélé quatre problèmes : les rapports étaient trop longs, le format contenait des emojis et de l'ASCII art, Alfred ajoutait des recommandations non demandées, et la réconciliation n'avait été testée que sur deux documents.

**Cycle 2 (TN3) :** cinq tests structurés sur des tâches distinctes, avec clés de correction prédéfinies. Chaque test est évalué sur cinq métriques transversales : détection (nombre d'éléments clés identifiés par rapport à la clé), faux positifs (éléments incorrectement signalés), concision (longueur de la sortie), fidélité factuelle (absence d'informations inventées) et format professionnel (absence d'emojis et de formatage inadapté).

Les quatre problèmes du cycle 1 ont été résolus par des ajustements du prompt système :

| Problème | Description | Solution | Résultat |
|----------|-------------|----------|----------|
| Longueur | Rapports trop longs pour un cadre dirigeant | Contrainte d'une page maximum dans le prompt | 5 tests conformes |
| Formatage | Emojis et ASCII art inadaptés | Template HTML figé avec CSS imposé | Zéro emoji en sortie |
| Recommandations | Ajouts non demandés de conseils stratégiques | Clause de zéro recommandation dans `<content_rules>` | Mode constat respecté |
| Multi-documents | Réconciliation testée sur 2 documents seulement | Tests avec 3, 8 et 10 documents | Fonctionne jusqu'à 10 documents |

---

## 3. Résultats

Cinq tests structurés ont été réalisés pour évaluer les capacités d'Alfred sur des tâches distinctes d'analyse documentaire. Chaque test utilise un sous-ensemble du corpus de 38 documents et dispose d'une clé de correction prédéfinie.

### 3.1 Description des tests

| Test | Objectif | Documents | Clé de correction |
|------|----------|-----------|------------------|
| Test 1 : Réconciliation COMEX | Détecter les divergences entre 3 comptes rendus d'une même réunion | 3 | 10 divergences attendues |
| Test 2 : Traçabilité infrastructure | Reconstituer l'historique d'un chiffre modifié sans justification | 3 | 5 éléments attendus |
| Test 3 : Chronologie Hugo | Reconstituer le parcours d'une personne à travers 8 documents | 8 | 8 jalons + 4 constats |
| Test 4 : Décisions réelles vs annoncées | Distinguer décisions prises et discussions en cours | 2 | 4 éléments minimum |
| Test 5 : Synthèse exécutive | Synthèse concise de 10 documents | 10 | 14 éléments, max 800 mots |

### 3.2 Métriques clés

| Test | Détection | Faux pos. | Concision | Fidélité | Format | Verdict |
|------|-----------|-----------|-----------|----------|--------|---------|
| T1 : Réconciliation | 7-8 / 10 | 0 | 1 page | Oui | Oui | Réussi |
| T2 : Traçabilité | 5 / 5 | 0 | 1/2 page | Oui | Oui | Réussi+ |
| T3 : Chronologie | 8/8 + 4/4 | 0 | 1 page | Oui | Oui | Réussi+ |
| T4 : Décisions | 4+ / 4 | 0 | 1 page | Oui | Oui | Réussi+ |
| T5 : Synthèse | 13-14 / 14 | 0 | 1 page | Oui | Oui | Réussi |

Alfred dépasse les seuils de réussite sur les cinq tests. Le taux de détection global est supérieur à 80 %. Aucun faux positif n'a été identifié. Toutes les sorties respectent le format professionnel et la contrainte de concision d'une page maximum. Aucune information inventée ou déformée n'a été constatée.

### 3.3 Résultats au-delà des attentes

Sur les tests 2, 3 et 4, Alfred produit des analyses qui vont au-delà de la clé de correction. Le test 2 (traçabilité) inclut un calcul des écarts en pourcentage avec un niveau de risque associé à chaque modification. Le test 3 (chronologie) ajoute un tableau comparatif entre les engagements annoncés et la réalité documentée. Le test 4 (décisions) propose une classification des divergences par niveau de gravité.

Ces ajouts améliorent la lisibilité sans dégrader la concision. Ils n'étaient pas demandés dans le prompt. Le modèle a trouvé seul des façons utiles de structurer l'information, dans le cadre posé par le prompt système.

---

<img width="781" height="780" alt="image" src="https://github.com/user-attachments/assets/f919105d-7432-47dd-9613-c4522cdc33c5" />

---

<img width="695" height="938" alt="image" src="https://github.com/user-attachments/assets/2fd18b83-981c-4637-a63b-2263dfd04437" />

---

*La vidéo de démonstration (section 7) montre Alfred en action sur un cas concret.*

---

## 4. Efficacité en entreprise

### 4.1 Comparaison avant / après

Le recoupement manuel du corpus de 38 documents a nécessité entre quatre et six heures de travail. Ce travail a permis d'identifier plusieurs contradictions (chiffres arrondis à la baisse, réserves omises, décisions présentées comme acquises), mais de manière non exhaustive. Comme noté dans le TN2 : « par manque de temps et en raison du volume de documents, je suis certainement passé à côté d'autres incohérences ».

Alfred produit une analyse comparable en quelques minutes. Sur le test 1 (réconciliation de trois comptes rendus du même comité de direction), il détecte entre 7 et 8 divergences sur les 10 répertoriées dans la clé de correction. Sur le test 5 (synthèse de 10 documents), il identifie 13 à 14 éléments sur les 14 attendus.

| Dimension | Recoupement manuel | Alfred |
|-----------|-------------------|--------|
| Temps d'analyse | 4 à 6 heures | Quelques minutes par test |
| Couverture | Partielle, avec angles morts reconnus | Supérieure à 80 % de la clé de correction |
| Faux positifs | Non mesurés | Zéro sur les 5 tests |
| Format de sortie | Notes personnelles non structurées | Rapport HTML d'une page, lisible en 2-3 minutes |
| Reproductibilité | Variable selon la charge cognitive | Stable, indépendante de la fatigue |

### 4.2 Réponse à la problématique

Alfred répond aux trois objectifs opérationnels du projet. Il réduit le temps d'analyse en passant de plusieurs heures de recoupement manuel à quelques minutes de traitement automatisé. Il améliore la lisibilité en produisant des rapports structurés avec un code couleur fixe et une contrainte d'une page. Il détecte systématiquement les contradictions entre sources, y compris celles que le recoupement manuel avait manquées.

L'assistant ne remplace pas le jugement humain. Il pose les faits sur la table : points de vigilance, désaccords internes, informations manquantes. Le décideur décide.

### 4.3 Adoption et limites

Alfred est déployé sur infrastructure locale et sera utilisé dans mon activité professionnelle au-delà du cadre de ce cours. L'auto-hébergement via Clawdbot permet un déploiement sans dépendance SaaS, avec contrôle total des données.

Trois limites conditionnent l'adoption en contexte réel :

- **Fenêtre de contexte :** le volume de documents analysables en une seule passe est limité par la fenêtre de contexte du modèle de langage. La stratégie retenue est l'injection sélective des documents pertinents plutôt que l'injection en masse.
- **Dépendance au prompt :** la qualité des sorties dépend directement de la précision du prompt système. Un prompt mal calibré produit des rapports trop longs, hors format ou contenant des recommandations non demandées.
- **Coût API :** chaque analyse implique un appel à l'API Anthropic, facturé à l'usage. Le coût reste modéré pour un usage individuel mais deviendrait significatif à l'échelle d'une équipe.

---

## 5. Difficultés et solutions

Cinq obstacles ont été rencontrés au cours du projet. Les quatre premiers ont été identifiés lors du rapport mi-parcours (TN2). Le cinquième a été découvert lors des tests finaux.

| Obstacle | Description | Solution appliquée | Statut |
|----------|-------------|-------------------|--------|
| Longueur des sorties | Les rapports d'Alfred étaient trop longs pour un cadre dirigeant en surcharge informationnelle. | Contrainte de format dans le prompt : synthèse d'une page maximum. Validée sur 5 tests. | Résolu |
| Formatage inadapté | Emojis et ASCII art dans les sorties, inadaptés au contexte professionnel. | Template HTML figé avec CSS imposé. Aucune modification de mise en forme possible. | Résolu |
| Réconciliation limitée | Le TN2 n'avait testé la réconciliation que sur 2 documents. Le cas central (3 versions d'un même COMEX) restait à tester. | Tests avec 3 documents (test 1), 8 documents (test 3) et 10 documents (test 5). | Résolu |
| Recommandations non sollicitées | Alfred ajoutait des recommandations stratégiques non demandées, en contradiction avec le positionnement du TN1. | Clause de zéro recommandation dans le prompt, avec refus explicite même sur demande du Directeur. | Résolu |
| Fenêtre de contexte | Le volume de documents injectables est limité par la fenêtre de contexte du modèle. | Injection sélective des documents pertinents. Stratégie documentée dans le guide d'utilisation. | Géré (limite connue) |

La progression entre le TN2 et le produit final montre que les obstacles identifiés étaient des problèmes de calibrage, pas d'architecture. Ils ont été traités par la conception d'un prompt système structuré en huit blocs, chaque bloc répondant à un problème spécifique. Le cinquième obstacle (fenêtre de contexte) est une limite technique du modèle de langage, gérée par une stratégie d'injection sélective mais non supprimée.

---

## 6. Réflexion sur les apprentissages

### 6.1 Compétences acquises

La compétence technique centrale du projet est l'ingénierie de prompts. Le point de départ était une instruction en langage naturel. Le résultat est un prompt système en huit blocs XML, avec des règles explicites, des types de rapport définis et une checklist de validation. Chaque bloc répond à un problème concret identifié en test. La structuration en XML permet de modifier un aspect du comportement sans toucher aux autres.

L'évaluation critique des sorties est l'autre compétence clé. Un modèle de langage peut produire des sorties convaincantes mais fausses. Sans clé de correction prédéfinie et sans métriques, il est impossible de distinguer une bonne analyse d'une hallucination bien formulée.

La constitution du corpus Teranga (38 documents, 10 acteurs, 4 périodes, ambiguïtés volontaires) a nécessité un travail de scénarisation qui reproduit les conditions réelles d'une négociation commerciale. Un corpus artificiel ou simpliste ne permet pas d'évaluer la capacité d'un assistant à gérer l'ambiguïté.

Le déploiement de Clawdbot sur infrastructure locale, la configuration OAuth, la gestion des appels API et le contrôle des accès constituent une expérience concrète de mise en œuvre d'une solution IA en dehors des plateformes SaaS.

### 6.2 Valeur professionnelle

En tant que directeur des systèmes d'information, ces compétences renforcent ma capacité à évaluer et déployer des solutions IA avec discernement. Le projet a produit un outil que j'utilise déjà. Mais il a aussi construit une compréhension concrète des limites des modèles de langage, loin du discours marketing.

La capacité à structurer un prompt, à tester les sorties de manière rigoureuse et à identifier les cas où le modèle échoue est une compétence rare dans les fonctions de direction. Elle permet de distinguer les cas d'usage où l'IA apporte une valeur réelle de ceux où elle crée une illusion de fiabilité.

### 6.3 Leçons tirées

**L'IA ne remplace pas le jugement.** Alfred structure l'information et détecte les contradictions. Il ne décide pas. La valeur de l'outil réside dans sa capacité à lever les angles morts du décideur, pas à se substituer à lui. Cette distinction, posée dès le TN1, a été confirmée tout au long du projet.

**La qualité du prompt détermine la qualité de la sortie.** Les quatre problèmes identifiés au TN2 (longueur, formatage, recommandations, multi-documents) ont tous été résolus par des ajustements du prompt, pas par un changement d'architecture. L'investissement dans la conception du prompt est le levier principal de qualité pour un assistant IA.

**Un corpus réaliste est indispensable.** Tester un assistant IA sur des documents simplistes donne des résultats trompeurs. Le corpus Teranga, avec ses contradictions subtiles et ses ambiguïtés volontaires, a permis d'évaluer Alfred dans des conditions proches de la réalité professionnelle.

**L'auto-hébergement a un coût.** Le déploiement local offre un contrôle total, mais il exige une compétence technique et un investissement en temps que toutes les organisations ne peuvent pas assumer. Le choix entre SaaS et auto-hébergement dépend du niveau de contrôle requis sur les données et du niveau de compétence disponible.

---

## 7. Présentation vidéo

Une vidéo de démonstration accompagne ce rapport. Elle montre Alfred en action sur un cas concret du corpus Teranga, de l'injection des documents à la production du rapport d'analyse.

La vidéo couvre les éléments suivants :

- Présentation du problème : surcharge informationnelle et contradictions entre sources.
- Environnement technique : Clawdbot déployé sur Mac Mini, connexion API Anthropic.
- Démonstration : injection de documents et production d'un rapport de réconciliation.
- Analyse de la sortie : lecture commentée du rapport HTML produit par Alfred.

[Lien vers la vidéo de démonstration](https://www.dropbox.com/scl/fi/mtmjk0vc8dkd0p9rl9qgx/Screen-Recording-02-03-2026-09-38-27.mp4?rlkey=31r5qs8nij75garyz91dpqsbp&st=gszmp7pp&dl=0)

---

## 8. Conclusion

Alfred répond à la problématique identifiée en début de projet : la surcharge informationnelle des cadres dirigeants et le risque de passer à côté d'informations clés dans des documents contradictoires.

Les cinq tests réalisés sur un corpus de 38 documents montrent un taux de détection supérieur à 80 %, zéro faux positif et une concision respectée sur chaque sortie. Les quatre limites identifiées au rapport mi-parcours ont été résolues par la conception d'un prompt système structuré en huit blocs. L'assistant est opérationnel et sera utilisé dans mon activité professionnelle.

Trois axes d'évolution sont envisagés : l'intégration directe avec des outils de messagerie et de gestion documentaire pour automatiser l'injection des documents ; l'ajout d'un mode de suivi longitudinal pour détecter les dérives entre les engagements pris et les actions réalisées au fil du temps ; l'exploration de stratégies de découpage pour traiter des corpus dépassant la fenêtre de contexte du modèle.

Le projet a produit un outil fonctionnel, mais il a aussi développé une compréhension concrète de ce que l'IA peut et ne peut pas faire dans un contexte d'aide à la décision. Cette compréhension est, à terme, plus précieuse que l'outil lui-même.
