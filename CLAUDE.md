# Instructions pour Claude Code

Ce dépôt utilise un système d'orchestration multi-agents :

- **Orchestrateur** : `.claude/skills/orchestrateur/SKILL.md` (invocable
  via `/orchestrateur`). Pour toute demande complexe ou multi-domaines,
  invoque cette skill : elle route la requête vers le meilleur agent
  spécialisé et recrute un nouvel agent si aucun ne convient.
- **Agents spécialisés** : `.claude/agents/` (voir le README de ce
  dossier pour le format et le cycle de vie des agents).

Pour une question simple et directe, réponds directement sans passer par
l'orchestrateur. Réponds en français par défaut.
