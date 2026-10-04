# Extraction de noyau et contre-passe

## Extraction de noyau

Après la génération, lance un sous-agent ingénieur de faisabilité, isolé lui aussi (`Agent`, `subagent_type: general-purpose`, jamais un fork). Spécialité du domaine principal du plan. Il reçoit le plan, les failles et uniquement les propositions des profils CRÉATIF et WTF. Il ne voit pas les propositions ÉPROUVÉ, PRAGMATIQUE et SYSTÉMIQUE, sinon il ramènerait tout vers elles.

Brief à lui transmettre, avec le plan et les failles au format du gabarit des profils :

---
Tu es [spécialité du domaine principal]. Tu es ingénieur de faisabilité. Tu ne lances aucun sous-agent. Tu réponds en français.

Pour chaque proposition reçue :
1. Énonce en une phrase le principe qui fait marcher l'idée, indépendamment de sa faisabilité.
2. Cherche une version de ce principe qui respecte les contraintes déclarées et les éléments actés.
3. Si tu en trouves une, rédige-la au format de solution ci-dessous, profil « NOYAU CRÉATIF » ou « NOYAU WTF ».
4. Si tu n'en trouves pas, écris « aucun noyau » et dis quelle contrainte rend le principe inapplicable.
5. Si le noyau extrait n'est qu'une pratique standard déjà connue, dis-le. Aucune fausse originalité.

Distinction à appliquer : une contrainte déclarée par l'utilisateur est une contrainte. Une habitude, une hypothèse implicite ou une façon de faire non déclarée n'en est pas une. Une proposition WTF qui ne brise qu'une hypothèse implicite peut être recevable telle quelle. Signale-le explicitement.

[Format de solution : copie du bloc <output> du gabarit des profils.]

[Propositions CRÉATIF et WTF, texte brut.]
---

## Contre-passe

Une seule contre-passe, sur l'ensemble des solutions retenues. Elle existe parce que tu évalues le travail de ta propre équipe, et qu'une parade crée souvent une nouvelle faille.

Lance deux sous-agents attaquants, isolés l'un de l'autre, en parallèle :
- PRÉ-MORTEM, spécialité du domaine principal : les solutions retenues ont été appliquées et le plan a quand même échoué. Pourquoi ?
- CASCADE, spécialité étrangère au domaine principal : quelles solutions retenues interagissent mal entre elles, se contredisent, ou créent une dépendance en série ?

Chacun reçoit le plan, l'objectif, les contraintes et l'ensemble des solutions retenues, et ce brief :

« Ne rends que les failles fatales ou sérieuses introduites par ces solutions ou par leurs interactions. Chaque faille doit passer quatre tests : un mécanisme explicable étape par étape, un déclencheur plausible dans l'horizon du plan appuyé sur un précédent ou un signal observable en cours, une spécificité réelle à ce plan, un effet sur l'objectif. Une faille qui échoue à un test ne se rend pas. Si tu ne trouves rien de sérieux, écris "aucune faille sérieuse". Tu ne proposes aucune solution. Tu ne lances aucun sous-agent. Tu réponds en français. »

Traitement des résultats :
- Faille fatale sur une solution retenue : elle est remplacée par son suppléant, marqué « non contre-vérifié ».
- Conflit entre deux solutions retenues : la solution qui traite la faille à l'impact le plus élevé est conservée. À impact égal, celle qui traite la faille à la probabilité la plus forte. L'autre est remplacée par son suppléant, marqué « non contre-vérifié ».
- Faille sérieuse : la solution est conservée et la faille est signalée dans le rapport.
- Pas de suppléant disponible : la faille d'origine passe en « aucune solution recevable ».

La contre-passe ne se relance jamais. Chaque tour supplémentaire rabote les solutions les plus originales jusqu'à ne laisser que la plus fade.
