# Protocole du vault

Le vault Obsidian « Coffre-fort » est la mémoire de la cellule. Un modèle n'apprend rien d'une exécution à l'autre. Seul ce qui est écrit puis relu se capitalise.

## Configuration

```
VAULT_DIR: non configuré
```

Remplace `non configuré` par le chemin absolu du dossier de la cellule dans le vault, par exemple `/Users/<toi>/Coffre-fort/Blue-Team`. Tant que la valeur est `non configuré`, la cellule se comporte comme sans accès.

## ACCÈS

Avant toute lecture ou écriture, vérifie que tu disposes d'un accès réel aux fichiers du vault : le chemin `VAULT_DIR` existe et se liste avec `Glob`. Dans une session cloud, le vault local de l'utilisateur n'est pas accessible. Sans accès, dis-le en première ligne du rapport, saute la lecture, et livre les notes en blocs markdown à coller. Tu n'écris jamais « note enregistrée » si tu ne l'as pas écrite.

## LECTURE, avant la génération

1. Cherche dans le dossier (`Grep` sur les champs `domaine`, `mecanisme`, `tags`) les notes dont le domaine ou le mécanisme ressemble aux failles retenues.
2. Transmets les notes trouvées au seul profil ÉPROUVÉ, et garde-les pour l'évaluation. Les autres profils ne les reçoivent pas. Des précédents partagés feraient converger toute l'équipe vers l'ancienne solution.
3. Règle de valeur : seule une note au statut « résultat observé » est un retour d'expérience. Une note « proposée », « adoptée » ou « rejetée » est une recommandation antérieure non testée. Elle ne prouve rien. La cellule ne se cite jamais elle-même comme preuve.
4. Les faits datés d'une note sont historiques. Ils repassent par le protocole de vérification avant toute réutilisation.

## ÉCRITURE, après le rapport

Une note par faille traitée. Avant de créer, cherche une note existante. Fusion seulement si le domaine et le mécanisme concordent tous les deux : tu ajoutes alors une section « Occurrence [date] ». En cas de doute, tu crées une nouvelle note avec un lien [[nom de la note voisine]]. Une fusion abusive corrompt l'historique sans que personne ne le voie.

Nom de fichier : `AAAA-MM-JJ_[domaine]_[faille en 3 à 5 mots].md`

Contenu :

```
---
type: blue-team
date: AAAA-MM-JJ
plan: [intitulé court du plan]
domaine: [domaine]
faille: [faille en une phrase]
mecanisme: [mécanisme en une phrase]
impact: fatal | sérieux | mineur
profil_retenu: [profil]
solution_adoptee: non renseignée
statut: proposée
resultat: non observé
faits_dates: [fait, valeur, statut, date de consultation]
tags: [blue-team, domaine, mot-clé du mécanisme]
---
## Solution retenue
[solution, point d'action, coût, délai, réversibilité]
## Pourquoi
[deux phrases]
## Écartées
[une ligne par proposition, avec le motif]
## Noyau
[ce que l'extraction a tiré des profils CRÉATIF et WTF]
## Résultat
[vide à la création]
```

## CYCLE DE VIE

Le statut ne change que sur déclaration explicite de l'utilisateur. Jamais de ta propre initiative.

- « J'applique la solution retenue » : statut adoptée, solution_adoptee renseignée.
- « J'applique une solution écartée » : statut adoptée, solution_adoptee renseignée avec la solution réellement appliquée, mention « contre la recommandation » et motif de rejet initial. Le résultat observé dira quel choix était le bon.
- « La contrainte X ne tient plus » : ce n'est pas une adoption, c'est un fait nouveau. Tu réappliques la grille selon la règle de contestation.
- « Je ne l'applique pas » : statut rejetée, avec la raison donnée.
- « Résultat : [réussite, échec, partiel] » : statut résultat observé, resultat renseigné, section Résultat remplie avec ce que l'utilisateur rapporte.

Pour une mise à jour de statut, tu ne relances aucune équipe. Tu modifies la note et tu confirmes en une ligne. Sans accès au vault, tu livres la note modifiée en bloc markdown à coller.
