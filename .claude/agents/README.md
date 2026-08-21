# Registre des agents spécialisés

Ce dossier est le « vivier » d'agents de l'orchestrateur (`/orchestrateur`,
défini dans `.claude/skills/orchestrateur/SKILL.md`). Chaque fichier `.md`
définit un agent spécialisé que Claude Code peut invoquer comme sous-agent.

## Format d'un agent

```markdown
---
name: nom-en-kebab-case
description: Spécialité de l'agent et QUAND l'utiliser. C'est sur ce texte
  que l'orchestrateur se base pour router les requêtes — sois précis.
tools: Read, Glob, Grep        # le strict nécessaire (moindre privilège)
model: sonnet                  # le modèle au meilleur ROI pour la tâche
---

Prompt système de l'agent : son rôle, sa méthode de travail, ses critères
de qualité et le format de sortie attendu.
```

## Choix du modèle (optimisation du ROI)

Chaque agent déclare le modèle le moins cher qui fait le travail au niveau
de qualité requis — on ne paie la puissance que là où elle rapporte :

| Modèle | Coût | Quand l'utiliser |
|---|---|---|
| `haiku` | € | Tâches mécaniques et volumineuses : extraction, lecture de PDF, résumé factuel, tri, reformatage, vérifications simples |
| `sonnet` | €€ | Défaut polyvalent : usage d'outils, recherche et filtrage avec jugement, rédaction courante, analyse standard |
| `opus` | €€€ | Raisonnement complexe ou livrable à fort enjeu et faible volume : rédaction stratégique (CV, candidature décisive), arbitrages difficiles, synthèse multi-sources critique |

Règles :
- En cas de doute entre deux modèles, prendre le moins cher et ne monter
  en gamme que si la qualité s'avère insuffisante.
- Un même agent peut être invoqué avec un modèle supérieur ponctuellement
  (paramètre `model` de l'outil Agent) quand l'enjeu de la tâche le
  justifie — sans changer son fichier.

## Cycle de vie

- **Embauche** : l'orchestrateur crée un nouveau fichier ici quand aucun
  agent existant ne couvre la spécialité demandée, **ou quand un agent en
  place n'est pas optimisé ou efficient** (mauvais modèle, outils
  inadaptés, méthode insuffisante) et qu'un profil mieux calibré fait
  mieux. Les agents créés en cours de session sont enregistrés au
  démarrage de la session suivante ; entre-temps l'orchestrateur les
  exécute via `general-purpose`.
- **Évolution** : améliorer un agent = éditer son fichier (affiner la
  description pour un meilleur routage, recalibrer le modèle ou les
  outils, enrichir sa méthode). À préférer au remplacement quand la
  spécialité reste la bonne.
- **Départ** : supprimer le fichier d'un agent devenu inutile ou rendu
  redondant par une meilleure recrue, et mettre à jour le tableau
  ci-dessous.

## Agents actuels

| Agent | Spécialité | Modèle | Justification ROI |
|---|---|---|---|
| `redacteur-cv` | CV, lettres de motivation, optimisation ATS | `opus` | Fort enjeu (candidatures), faible volume : la qualité rédactionnelle paie directement |
| `chercheur-emploi` | Recherche et analyse d'offres d'emploi / missions | `sonnet` | Usage d'outils et jugement d'adéquation ; volume moyen |
| `documentaliste` | Lecture, synthèse et organisation des documents du dépôt | `haiku` | Extraction et synthèse factuelles, volumineuses, peu risquées |
