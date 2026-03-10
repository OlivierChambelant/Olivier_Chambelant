# Projet d'intégration de l'intelligence artificielle dans les entreprises

---

## 1. Analyse du problème

### 1.1 Description du problème

Les cadres dirigeants traitent chaque jour un volume important de documents professionnels. Comptes rendus de réunions, courriels, documents de cadrage, propositions commerciales. Ces documents sont rédigés par des personnes différentes, avec des priorités et des perspectives distinctes.

Une même réunion peut produire plusieurs comptes rendus. Chacun reflète le point de vue de son auteur. Les informations critiques sont noyées dans le bruit. Les décisions sont parfois présentées comme acquises alors qu'elles ne le sont pas. Les risques signalés par un acteur technique peuvent être absents du compte rendu officiel.

Le problème est double. Le temps consacré à lire, croiser et analyser ces documents est considérable et le risque de passer à côté d'une information clé est réel.

### 1.2 Contexte et importance

Ce projet s'inscrit dans mon contexte professionnel. Je suis confronté à cette surcharge informationnelle de manière récurrente. Je traite régulièrement des volumes importants d'informations hétérogènes nécessitant une synthèse fiable et exploitable.

L'enjeu n'est pas d'automatiser la prise de décision. L'enjeu est de fournir un outil d'aide à l'analyse qui structure l'information, identifie les contradictions entre sources et met en évidence les éléments qui requièrent l'attention du décideur. La responsabilité de la décision reste humaine.

### 1.3 Analyse effectuée

#### Corpus de test

Pour évaluer la capacité de l'assistant, j'ai constitué un corpus de test à partir de documents professionnels anonymisés. Les documents sont issus d'une négociation commerciale impliquant plusieurs directions avec des intérêts divergents.

Le contexte implique une dizaine d'acteurs à différents niveaux hiérarchiques. Un dirigeant qui pousse pour signer. Une direction technique qui protège la qualité du produit. Une direction RH qui défend les conditions de ses équipes. Une direction des systèmes d'information prise en étau entre les trois. Et leurs équipes respectives, qui reçoivent des consignes parfois contradictoires.

Le corpus comprend 37 documents répartis sur quatre périodes chronologiques :

- **Premier contact et qualification**
  - Courriels d'estimation, expression de besoin client, échanges internes.
- **Cadrage interne**
  - Trois versions d'un même comité de direction, alertes techniques, échanges RH.
- **Réunion client et négociation**
  - Compte rendu officiel et version interne, révision budgétaire, réponses des directions.
- **Arbitrage et proposition**
  - Proposition commerciale, cahier des charges technique, réactions internes.

#### Ambiguïtés constatées

Après un travail de recoupement, j'ai identifié plusieurs ambiguïtés et contradictions dans le corpus. Des chiffres arrondis dans le mauvais sens. Des réserves omises dans les comptes rendus officiels. Trois versions d'une même réunion avec des faits différents. Des communications formelles contredites par des échanges internes. Un montant modifié entre l'estimation interne et la proposition commerciale sans traçabilité.

Ce travail de recoupement manuel est long et incomplet. Par manque de temps et en raison du volume de documents, je suis certainement passé à côté d'autres incohérences. C'est précisément l'objectif d'« Alfred » : lever ces angles morts en croisant systématiquement les sources, sans contrainte de temps ni de charge cognitive.

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

---

## 6. Annexe
<img width="975" height="731" alt="image" src="https://github.com/user-attachments/assets/f13da4d8-37b6-4f82-8c44-f354ea9bef17" />

### Rapport de synthèse du document : « Opportunité majeure — Groupe Teranga Santé »

Monsieur,

Voici la synthèse demandée du document T1-01 (courriel Marc Delisle).

═══════════════════════════════════════════════════════
  SYNTHÈSE T1-01 — Email Marc Delisle : Opportunité Teranga Santé
═══════════════════════════════════════════════════════

  DÉCISION
───────────

Décision prise (Marc Delisle) :
• Poursuivre activement l'opportunité commerciale avec le Groupe Teranga Santé
• Monter une offre complète (licence + intégration + expert détaché)
• Mobiliser l'équipe sur une réponse rapide (deadline : mercredi)

Portée stratégique :
• Enjeu majeur : Premier contrat dans le secteur santé privé québécois
• Effet de levier : Signature = porte d'entrée vers l'ensemble du réseau privé


  ACTIONS
──────────

Responsable          Périmètre                      Livrable
     Deadline
─────────────────────────────────────────────────────────────────────────────────────
Sophie Tremblay      Licence + personnalisations    Estimation
périmètre    Mercredi
Antoine Gauthier     Détachement expert + formation Estimation
périmètre    Mercredi
Olivier Chambelant   Déploiement + intégration      Estimation
périmère     Mercredi
COMEX (tous)         Consolidation offre            Discussion
stratégique  Jeudi matin

Contrainte temporelle client : Démarrage projet avant septembre 2025


  RISQUES
──────────

  RISQUES CRITIQUES

1. Timing ultra-serré
   - Estimation complète en 2 jours (mercredi)
   - Démarrage avant septembre → ~4-5 mois max pour signer + préparer
   - Risque de précipitation → sous-estimation / sur-promesse

2. Manque d'expérience métier santé
   - Premier contrat dans le secteur
   - Spécificités réglementaires santé (RAMQ, ordres professionnels, protection
     données patients) potentiellement sous-estimées

3. Client "pressé" (red flag)
   - Pression sur délais → risque de sacrifier qualité
   - Raison de l'urgence non explicitée (départ fournisseur actuel ?
crise interne ?)

  RISQUES MODÉRÉS

4. Complexité organisationnelle
   - 6 000 employés répartis sur plusieurs cliniques
   - Intégration multi-sites + gestion changement complexe

5. Dépendance stratégique
   - "Si on signe Teranga..." → risque de surinvestissement commercial
   - Pression implicite sur l'équipe pour accepter des conditions défavorables

6. Absence de cadrage formel
   - Email basé sur "échange informel" avec VP Opérations
   - Pas encore d'expression de besoin formelle
   - Périmètre réel flou ("suite complète")


  RECOMMANDATION
─────────────────

Vous devrez estimer le déploiement/intégration SANS cahier des charges détaillé
→ forte incertitude.

Suggérer de demander un minimum de cadrage avant mercredi ou de
prévoir des marges
de sécurité importantes dans l'estimation.

═══════════════════════════════════════════════════════

Cordialement,
Alfred  


---

### Rapport de synthèse de la réconciliation des documents

Monsieur,

Voici l'analyse de réconciliation des trois documents relatifs au
projet Teranga Santé.

═══════════════════════════════════════════════════════════════════════════
  RÉCONCILIATION DES DOCUMENTS — Projet Teranga Santé
═══════════════════════════════════════════════════════════════════════════

  DOCUMENTS ANALYSÉS
─────────────────────

1. T1-01 : Email Marc Delisle (7 avril) — Annonce opportunité
2. CR officiel : Compte-rendu COMEX par Marc (10 avril)
3. CR technique : Compte-rendu COMEX par Sophie (10 avril)


  DIVERGENCES CRITIQUES
─────────────────────────

  MONTANTS CONTRADICTOIRES

Poste                              CR Marc        CR Sophie      Écart
─────────────────────────────────────────────────────────────────────────
Licence + personnalisations       1 050 000 $    1 085 000 $    -35 000 $
Détachement + formation             360 000 $      360 000 $     
Déploiement + intégration           425 000 $      425 000 $     
─────────────────────────────────────────────────────────────────────────
TOTAL PROJET                      1 850 000 $    1 870 000 $    -20 000 $

  ANALYSE :
• Marc affiche 1 850 000 $ = budget client (présente le projet comme
"dans l'enveloppe")
• Sophie corrige : total réel = 1 870 000 $ = DÉPASSEMENT de 20 000 $
• Marc a déjà arrondi à la baisse l'estimation de Sophie (-35 k$) dans
son email du 9 avril


  STATUT DES DÉCISIONS

Sujet                    Version Marc                        Version Sophie
────────────────────────────────────────────────────────────────────────────────
Licence SaaS            "Sophie évalue la possibilité"      "Point NON TRANCHÉ"
                        (action décidée)
Détachement hybride     "Antoine optimise le modèle"        Antoine "N'A PRIS
                        (action décidée)                     AUCUN ENGAGEMENT"
Réduction budgétaire    "20 000 $ se trouvent toujours"     "AUCUNE
direction n'a
                                                             accepté
de réduction"

  ANALYSE :
• Marc transforme des EXPLORATIONS en ENGAGEMENTS
• Sophie documente que RIEN n'est décidé
• Pression commerciale (Marc) vs réalisme technique (Sophie)


  RISQUES TECHNIQUES OMIS PAR MARC

Risque                          Mentionné par Marc ?    Détails (Sophie)
────────────────────────────────────────────────────────────────────────────────
Interface MedPlan                 NON                  • 60 000 $
"hypothèse optimiste"
                                                        • "N'a JAMAIS
été réalisée"
                                                        • Coordination
CTO-CIO nécessaire
                                                        • "Risque NON CHIFFRÉ"

Conventions collectives santé     NON                  • 120 000 $
dans estimation
                                                        • "POURRAIT
AUGMENTER après analyse"

Licence SaaS (risque)             NON                  • "Réduit notre
revenu récurrent"
                                                        • "Crée un
RISQUE DE RÉSILIATION"

Disponibilité ressources          NON                  • Faisabilité
CONDITIONNÉE
                                                        • Expert à
identifier avant fin mai

  ANALYSE :
• Marc omet TOUS les risques techniques dans son CR officiel
• Sophie les documente méthodiquement
• MedPlan = bombe à retardement (jamais fait, mais facturé 60 k$)


  TENSIONS ORGANISATIONNELLES
───────────────────────────────

MARC (CEO) vs SOPHIE (CTO)
• Posture Marc : Optimisme commercial, minimisation des obstacles
• Posture Sophie : Prudence technique, documentation des risques
• Conflit : Marc corrige déjà les chiffres de Sophie avant négociation
(1 085 k$ → 1 050 k$)

MARC vs ANTOINE (DRH)
• Antoine REFUSE de négocier son tarif journalier (1 250 $)
• Antoine NE S'ENGAGE PAS sur le modèle hybride
• Marc présente quand même ces points comme "en cours d'optimisation"

MARC vs RÉALITÉ BUDGÉTAIRE
• Budget client : 1 850 000 $
• Total estimations : 1 870 000 $
• Solution de Marc : "20 000 $ se trouvent toujours" (attitude cavalière)
• Réalité (Sophie) : AUCUNE direction n'a accepté de baisser


  VOTRE POSITION (lecture entre les lignes)
─────────────────────────────────────────────

Dans le CR de Sophie :
• Vous proposez une solution technique (impliquer équipe Teranga sur
reprise données)
• MAIS conditionnez la faisabilité à la disponibilité des ressources
(expert à identifier
  avant fin mai)
• Signalez besoin de coordination CTO-CIO sur MedPlan

ANALYSE : Vous êtes COOPÉRATIF mais PRUDENT. Vous proposez des pistes
sans garantir
le résultat. Vous documentez les dépendances.


  RISQUES POUR VOUS
────────────────────

1. PRESSION POUR "TROUVER" LES 20 000 $
   • Marc présente l'écart comme mineur ("se trouve toujours")
   • Sophie/Antoine n'ont pas baissé leurs estimations
   • Risque : Vous serez le DERNIER REMPART → pression pour rogner sur
     déploiement/intégration

2. MedPLAN = BOMBE À RETARDEMENT
   • Jamais réalisé mais budgété à 60 k$ (optimiste)
   • Coordination CTO-CIO nécessaire
   • Si ça dérape, vous (CIO) serez co-responsable avec Sophie (CTO)

3. DISPONIBILITÉ RESSOURCES NON GARANTIE
   • Faisabilité conditionnée à identification d'un expert (RH) avant fin mai
   • Si Antoine (DRH) ne livre pas → risque de retard imputé à vous

4. SUR-PROMESSE COMMERCIALE
   • Marc minimise les risques pour signer
   • Si projet dérape, l'équipe technique (Sophie + vous) sera tenue responsable


  RECOMMANDATIONS
──────────────────

AVANT LE 16 AVRIL (deadline éléments révisés)

1. DOCUMENTER PAR ÉCRIT les hypothèses de votre estimation :
   • Périmètre exact de la reprise de données (avec/sans implication Teranga)
   • Disponibilité de l'expert (date butoir : fin mai)
   • Coordination CTO-CIO sur MedPlan (chiffrage à valider avec Sophie)

2. NE PAS ACCEPTER DE RÉDUCTION SANS CONTREPARTIE :
   • Si pression pour baisser les 425 k$, exiger un périmètre réduit
ou délai allongé
   • Sophie et Antoine n'ont pas cédé → vous non plus

3. FORMALISER LES DÉPENDANCES :
   • "Les 425 k$ sont conditionnés à la disponibilité de [nom expert]
avant le 31 mai"
   • "Le chiffrage MedPlan (60 k$) est sous responsabilité conjointe
CTO-CIO et doit
     être validé par Fatou avant proposition commerciale"

4. ALERTE SUR LE DÉCALAGE MARC/SOPHIE :
   • Marc arrondit déjà à la baisse (1 085 k$ → 1 050 k$)
   • Marc omet les risques techniques
   • PROTÉGEZ-VOUS : tout engagement doit être écrit et conditionné

PENDANT LE COMEX DU 16 AVRIL

5. ALIGNER AVEC SOPHIE SUR MedPLAN :
   • Confirmer publiquement que le chiffrage (60 k$) est "optimiste"
et "jamais réalisé"
   • Exiger validation Fatou avant proposition client

6. RAPPELER LA DÉPENDANCE RH :
   • "Antoine, as-tu identifié l'expert pour le détachement ? Sans
lui, le planning
     est caduc."

7. SI PRESSION POUR "TROUVER LES 20 K$" :
   • "Je peux étudier des optimisations SI le client accepte de
prendre en charge
     la reprise de données. Mais ça change le périmètre et nécessite
un engagement
     formel de leur part."


  SYNTHÈSE EXÉCUTIVE
─────────────────────

Aspect                      Constat
──────────────────────────────────────────────────────────────────────────
Alignement stratégique        Tous d'accord : priorité n°1
Alignement budgétaire         Écart 20 k$ non résolu + risques techniques
Alignement décisionnel        Marc présente des décisions qui n'existent pas
Alignement de risque          Marc omet tous les risques documentés par Sophie
Votre position                Coopératif mais prudent — protégez-vous

CONCLUSION : Marc pousse commercialement. Sophie résiste techniquement.
Vous êtes au milieu.

  DOCUMENTEZ TOUT, NE CÉDEZ RIEN SANS CONTREPARTIE, ALIGNEZ-VOUS AVEC SOPHIE.

═══════════════════════════════════════════════════════════════════════════

Cordialement,
Alfred  
