# Travaux universitaires — Alfred

Travaux réalisés dans le cadre du cours **Projet d'intégration de l'intelligence artificielle dans les entreprises**, programme court Intelligence artificielle dans les entreprises.

Ces travaux documentent la conception et le développement d'**Alfred**, un assistant IA personnel d'aide à l'analyse documentaire, déployé sur infrastructure locale via le framework open source Clawdbot.

---

## Problématique

Les cadres dirigeants traitent chaque jour un volume important de documents professionnels rédigés par des acteurs aux intérêts divergents. Une même réunion peut produire plusieurs comptes rendus contradictoires. Le recoupement manuel de ces sources est long, incomplet et expose au risque de passer à côté d'informations clés.

Alfred répond à ce problème en croisant systématiquement les sources, en détectant les contradictions factuelles et en produisant des synthèses structurées en quelques minutes.

---

## Contenu du dossier

| Fichier | Description |
|---------|-------------|
| `assistant-ia-analyse-documentaire-plan-de-projet.md` | Plan de projet : choix du projet, justification, portée, calendrier et ressources. |
| `assistant-ia-analyse-documentaire-prototype-et-tests-preliminaires.md` | Rapport mi-parcours : analyse du problème, prototype, résultats préliminaires et plan pour la suite. |
| `assistant-ia-analyse-documentaire-produit-final.md` | Rapport final : description du produit, mise en œuvre, tests structurés, guide d'utilisation et prompt système complet. |

---

## Alfred en bref

- **Framework :** Clawdbot (open source), déployé sur Mac Mini
- **Modèle :** Claude Sonnet 4.5 via API Anthropic (OAuth)
- **Corpus de test :** 38 documents anonymisés simulant une négociation commerciale complexe (10 acteurs, 4 périodes chronologiques)
- **Prompt système :** 8 blocs XML — rôle, contexte, sélection, sources, format, types de rapport, règles de contenu, checklist
- **Sorties :** rapports HTML d'une page, code couleur fixe, zéro recommandation par défaut

## Résultats des tests

| Test | Détection | Faux positifs | Verdict |
|------|-----------|---------------|---------|
| Réconciliation COMEX (3 CR) | 7-8 / 10 | 0 | Réussi |
| Traçabilité infrastructure | 5 / 5 | 0 | Réussi+ |
| Chronologie par acteur | 8/8 + 4/4 | 0 | Réussi+ |
| Décisions réelles vs annoncées | 4+ / 4 | 0 | Réussi+ |
| Synthèse exécutive (10 docs) | 13-14 / 14 | 0 | Réussi |

Taux de détection global supérieur à 80 %. Zéro faux positif sur l'ensemble des tests.

---

## Auteur

Olivier Chambelant — Directeur des systèmes d'information
