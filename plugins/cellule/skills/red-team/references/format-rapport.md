# Format du rapport Red Team

Le rapport suit exactement cette structure. C'est aussi le contrat d'entrée de `/cellule:blue-team`. Ne change ni les titres ni les champs.

1. ÉQUIPE : une ligne par sous-agent, méthode et spécialité.
2. ÉTAT DU PLAN : une phrase. Trois valeurs possibles : tient, fragile, ne tient pas. Suivie de la raison principale. Si plusieurs options sont attaquées, un état par option, sans classement entre elles. Si l'horizon n'a pas été fourni, précise que la plausibilité a été testée sur 12 mois.
3. FAILLES : classées et numérotées selon la consolidation (F1, F2, ou A-F1, B-F1 avec plusieurs options).
   - Fatales et sérieuses, développées : identifiant, faille, mécanisme, déclencheur, précédent ou signal, probabilité, impact, statut de vérification (vérifiée, non vérifiée, sans fait daté), sous-agents qui l'ont trouvée, pique éventuelle.
   - Mineures, en une ligne chacune : identifiant, faille, mécanisme, déclencheur, probabilité, statut de vérification. Le mécanisme et le déclencheur restent présents, sinon la Blue Team ne peut pas les traiter si l'utilisateur le demande.
   - Si plusieurs options, une sous-section par option.
4. HYPOTHÈSES IMPLICITES : ce que le plan suppose sans le démontrer.
5. ANGLES MORTS : les zones où l'équipe a manqué de compétence, et les zones non attaquées à la demande de l'utilisateur. Ce que le rapport ne couvre pas.
6. OBJECTIONS ÉCARTÉES : une ligne chacune, avec le test échoué ou le motif.
7. DONNÉES VÉRIFIÉES : tableau. Fait, valeur, source, statut, date de consultation.
8. HISTORISATION : identifiant du plan `[plan]`, date, fiches Red créées (titre et chemin), section d'index ajoutée, chemin de `rapport-red.md`. Chaque élément porte « enregistrée » seulement après relecture réussie. Sans accès au vault, les fiches et la section d'index en blocs markdown sous le titre « À coller dans Coffre-fort ».

Rien après le point 8. Pas de recommandation, pas de prochaine étape.

## Exemple positif de faille

« F1. FATAL, probabilité forte. Le plan suppose que le permis de travail arrive avant la date de prise de poste. Aucune marge n'est prévue entre le délai de traitement affiché et l'échéance. Déclencheur : un délai qui dépasse la moyenne de quelques semaines. Précédent : les délais affichés varient d'un mois à l'autre et dépassent régulièrement les standards de service. Mécanisme : l'employeur ne peut pas attendre, l'offre tombe, la séquence entière recule d'un cycle. Statut : vérifiée. Trouvée par PRÉ-MORTEM et CASCADE. Pique : un calendrier construit sur le meilleur délai jamais observé, c'est un pari, pas un plan. »

## Exemple négatif, objection vague

« Le calendrier semble un peu serré et pourrait poser problème. »

Ce format échoue : aucun mécanisme, aucun déclencheur, aucune cotation. L'utilisateur ne sait ni quoi surveiller ni à quel point s'inquiéter.

## Exemple négatif, objection absurde

« Une crise géopolitique majeure pourrait geler les programmes d'immigration et anéantir le projet. »

Ce format échoue : aucun précédent ni signal en cours sur l'horizon du plan, et l'objection s'appliquerait mot pour mot à n'importe quel projet d'expatriation. Elle échoue aux tests de plausibilité et de spécificité.
