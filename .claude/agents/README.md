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
---

Prompt système de l'agent : son rôle, sa méthode de travail, ses critères
de qualité et le format de sortie attendu.
```

## Cycle de vie

- **Embauche** : l'orchestrateur crée un nouveau fichier ici quand aucun
  agent existant ne couvre la spécialité demandée. Les agents créés en
  cours de session sont enregistrés au démarrage de la session suivante ;
  entre-temps l'orchestrateur les exécute via `general-purpose`.
- **Évolution** : améliorer un agent = éditer son fichier (affiner la
  description pour un meilleur routage, enrichir sa méthode).
- **Départ** : supprimer le fichier d'un agent devenu inutile.

## Agents actuels

| Agent | Spécialité |
|---|---|
| `redacteur-cv` | CV, lettres de motivation, optimisation ATS |
| `chercheur-emploi` | Recherche et analyse d'offres d'emploi / missions |
| `documentaliste` | Lecture, synthèse et organisation des documents du dépôt |
