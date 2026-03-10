## 1. Analyse du problème

### 1.1 Description du problème

Les cadres dirigeants traitent chaque jour un volume important de documents professionnels : comptes rendus de réunions, courriels, documents de cadrage, propositions commerciales. Ces documents sont rédigés par des personnes différentes, avec des priorités et des perspectives distinctes.

Une même réunion peut produire plusieurs comptes rendus. Chacun reflète le point de vue de son auteur. Les informations critiques sont noyées dans le bruit. Les décisions sont parfois présentées comme acquises alors qu'elles ne le sont pas. Les risques signalés par un acteur technique peuvent être absents du compte rendu officiel.

Le problème est double : le temps consacré à lire, croiser et analyser ces documents est considérable et le risque de passer à côté d'une information clé est réel.

### 1.2 Contexte et importance

Ce projet s'inscrit dans mon contexte professionnel. Je suis confronté à cette surcharge informationnelle de manière récurrente. Je traite régulièrement des volumes importants d'informations hétérogènes nécessitant une synthèse fiable et exploitable.

L'enjeu n'est pas d'automatiser la prise de décision. L'enjeu est de fournir un outil d'aide à l'analyse qui structure l'information, identifie les contradictions entre sources et met en évidence les éléments qui requièrent l'attention du décideur. La responsabilité de la décision reste humaine.

### 1.3 Analyse effectuée

#### Corpus de test

Pour évaluer la capacité de l'assistant, j'ai constitué un corpus de test à partir de documents professionnels anonymisés. Les documents sont issus d'une négociation commerciale impliquant plusieurs directions avec des intérêts divergents.

Le contexte implique une dizaine d'acteurs à différents niveaux hiérarchiques : un dirigeant qui pousse pour signer, une direction technique qui protège la qualité du produit, une direction RH qui défend les conditions de ses équipes, une direction des systèmes d'information prise en étau entre les trois, et leurs équipes respectives qui reçoivent des consignes parfois contradictoires.

Le corpus comprend 37 documents répartis sur quatre périodes chronologiques :

**Premier contact et qualification**
- Courriels d'estimation, expression de besoin client, échanges internes.

**Cadrage interne**
- Trois versions d'un même comité de direction, alertes techniques, échanges RH.

**Réunion client et négociation**
- Compte rendu officiel et version interne, révision budgétaire, réponses des directions.

**Arbitrage et proposition**
- Proposition commerciale, cahier des charges technique, réactions internes.

#### Ambiguïtés constatées

Après un travail de recoupement, j'ai identifié plusieurs ambiguïtés et contradictions dans le corpus : des chiffres arrondis dans le mauvais sens, des réserves omises dans les comptes rendus officiels, trois versions d'une même réunion avec des faits différents, des communications formelles contredites par des échanges internes, un montant modifié entre l'estimation interne et la proposition commerciale sans traçabilité.

Ce travail de recoupement manuel est long et incomplet. Par manque de temps et en raison du volume de documents, je suis certainement passé à côté d'autres incohérences. C'est précisément l'objectif d'Alfred : lever ces angles morts en croisant systématiquement les sources, sans contrainte de temps ni de charge cognitive.

La réalité d'une situation n'est contenue dans aucun document seul. Elle doit être reconstituée par croisement.

#### Méthode utilisée

Les documents sont déposés sur un partage réseau local. Alfred les récupère et les analyse via l'API Anthropic. Le recoupement manuel a été réalisé en amont pour identifier les contradictions les plus visibles.

#### Sources de données

Les documents sont anonymisés et ne contiennent aucune donnée confidentielle. Ils sont structurés en fichiers individuels, organisés par période chronologique, pour permettre l'injection sélective dans l'assistant.

---

## 2. Modélisation et prototype de la solution

### 2.1 Description de la solution

La solution repose sur Clawdbot, un framework open source d'assistant IA persistant. Il est déployé sur un Mac Mini dédié. J'ai configuré une instance nommée Alfred, qui agit comme mon assistant personnel d'aide à l'analyse documentaire.

Alfred analyse des documents professionnels semi-structurés. Il produit des synthèses structurées mettant en évidence décisions, actions, risques et priorités. Les modèles de langage sont accessibles via l'API Anthropic par connexion OAuth. Le traitement est orchestré depuis l'infrastructure locale.

### 2.2 Architecture

L'architecture comprend trois composants :

- **Entrée :** documents professionnels en format texte, injectés individuellement ou par lot.
- **Traitement :** Clawdbot orchestre les appels à l'API Anthropic. Le prompt système est configuré pour produire des analyses orientées aide à la décision.
- **Sortie :** rapports d'analyse structurés en format texte.

Les données restent sur l'infrastructure locale. Seul le contenu des documents est transmis à l'API pour traitement.

*Un diagramme d'architecture est fourni en annexe.*

### 2.3 Résultats préliminaires

Deux tests ont été réalisés pour évaluer les capacités d'Alfred.

#### Test 1 : analyse d'un document unique

J'ai injecté un courriel de direction annonçant une opportunité commerciale. Le courriel demandait des estimations à trois directions avec un délai de deux jours.

Alfred a produit un rapport structuré couvrant le contexte, les décisions, les actions et les risques. Il a correctement identifié qu'aucune décision n'était formalisée dans ce message. C'est un point important : l'assistant ne fabrique pas de fausses décisions. Il a également détecté six risques implicites, dont le délai trop court pour une estimation fiable, la pression calendaire et la prédominance de l'enjeu stratégique sur l'analyse technique.

#### Test 2 : réconciliation de deux documents

J'ai injecté le même courriel de direction et les notes internes d'un comité de direction tenu trois jours plus tard. Les notes avaient été rédigées par une cheffe de projet, avec un regard opérationnel.

Alfred a produit un rapport de réconciliation. Il a détecté plusieurs contradictions :

- Un effectif client indiqué à 6 000 par le CEO alors que le document client mentionne 6 200.
- Un chiffrage rappelé à 1 050 000 $ en réunion alors que le courriel d'origine indiquait 1 085 000 $.
- Un budget client présenté comme flexible alors qu'il était qualifié de ferme dans l'expression de besoin.
- Des discussions présentées comme des décisions par le CEO, alors qu'aucun directeur n'avait formellement accepté de réduction.
- Un risque calendaire non relevé : le premier lot devait être recetté en décembre, en période de congés.

Alfred a également produit une analyse des postures de chaque acteur, en identifiant les différences de ton et de positionnement entre les communications officielles et les échanges internes.

### 2.4 Évaluation préliminaire

Les deux tests montrent qu'Alfred extrait correctement les éléments structurants d'un document et détecte des contradictions entre deux sources. Trois limites sont identifiées à ce stade.

**Longueur des sorties.**
Le rapport de réconciliation est trop détaillé. L'objectif est d'améliorer la lisibilité pour les décideurs, pas de produire un nouveau document volumineux.

**Recommandations non demandées.**
Alfred ajoute des recommandations stratégiques à son analyse. La frontière entre constat factuel et prise de position n'est pas encore définie dans le prompt.

**Formatage.**
Les sorties contiennent des emojis et un formatage inadapté au contexte professionnel. Le prompt doit être ajusté.

---

## 3. Difficultés rencontrées et solutions envisagées

| Difficulté | Description | Solution envisagée |
|------------|-------------|-------------------|
| Longueur des synthèses | Alfred produit des rapports trop longs pour un cadre dirigeant en contexte de surcharge informationnelle. | Ajustement du prompt pour imposer un format condensé. Exploration d'un mode synthèse exécutive d'une page maximum. |
| Contrôle du format de sortie | Le modèle de langage ajoute des emojis et un formatage inadapté au contexte professionnel. | Contraintes de format intégrées au prompt système. Tests itératifs pour valider le respect des consignes. |
| Réconciliation multi-documents | Le test de réconciliation n'a porté que sur deux documents. Le cas central du projet, croiser trois versions d'une même réunion, n'a pas encore été testé. | Test prévu avec trois comptes rendus du même comité de direction pour évaluer la détection de contradictions subtiles. |
| Frontière analyse et recommandation | Alfred ajoute des recommandations stratégiques non demandées. La frontière entre aide à l'analyse et prise de position n'est pas définie. | Conception de deux modes de prompt : un mode constat (faits, contradictions, risques) et un mode conseil (avec recommandations). Choix selon le contexte. |

---

## 4. Plan pour la suite

### 4.1 Tests multi-documents

Le test prioritaire est la réconciliation de trois comptes rendus d'un même comité de direction, rédigés par trois personnes différentes. Ce test évaluera la capacité d'Alfred à détecter les contradictions subtiles entre des sources qui décrivent le même événement. Un second test portera sur l'injection d'un ensemble de documents couvrant une période complète.

### 4.2 Calibrage des prompts

Le prompt système sera ajusté pour contrôler la longueur des sorties, le format et le périmètre. L'objectif est de produire une synthèse exécutive d'une page, lisible en moins de trois minutes.

### 4.3 Évaluation de la qualité

Une grille d'évaluation sera définie pour mesurer la qualité des synthèses sur trois dimensions : la précision des décisions et actions extraites, la couverture des contradictions et risques détectés, et la concision du rapport produit.

### 4.4 Produit final

Le produit final inclura Alfred configuré et fonctionnel, le corpus de documents, les résultats d'analyse, un guide d'utilisation et une analyse critique des limites de l'assistant.

---

## 5. Conclusion

L'analyse du problème est complète. La surcharge informationnelle des cadres dirigeants, les contradictions entre sources et la difficulté à extraire les décisions réelles sont documentées dans un corpus de 37 documents anonymisés.

Le prototype est opérationnel. Alfred est installé sur infrastructure locale et connecté à l'API Anthropic. Les deux premiers tests montrent une capacité d'extraction correcte sur document unique et une détection de contradictions en mode réconciliation.

Les limites identifiées sont des problèmes de calibrage, pas d'architecture. Elles seront traitées par ajustement des prompts dans les semaines suivantes.

Le projet est en bonne voie pour livrer un produit final fonctionnel et documenté.
