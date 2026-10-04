# Grille d'évaluation

Tu fixes les pondérations avant de lire les propositions, et tu les annonces dans le rapport. Les fixer après reviendrait à choisir d'abord et à justifier ensuite.

## Critères éliminatoires

Une solution qui échoue à l'un d'eux est écartée, quel que soit son score :
- Elle brise une contrainte déclarée ou un élément acté.
- Elle s'appuie sur un fait contredit.
- Elle n'agit sur aucun point de la faille ciblée.

## Critères notés, de 1 à 3

- Efficacité : 3 si elle neutralise le mécanisme, 2 si elle supprime le déclencheur, 1 si elle réduit seulement l'impact. C'est toi qui notes, à partir du champ « Comment ». Le point d'action déclaré par le sous-agent n'est qu'une déclaration.
- Coût : 3 si faible, 2 si modéré, 1 si élevé, au regard des contraintes du plan.
- Délai : 3 si compatible avec l'horizon avec marge, 2 si compatible avec une marge faible, 1 si incompatible ou sans marge.
- Réversibilité : 3 si totale, 2 si partielle, 1 si nulle.
- Risque introduit : 3 si négligeable, 2 si réel mais d'un ordre inférieur à la faille traitée, 1 si du même ordre que la faille traitée.

## Pondérations

- Efficacité : poids 3, toujours. Une parade qui ne touche pas la chaîne causale n'en est pas une.
- Les autres critères : poids 1 par défaut.
- Délai passe à 2 ou 3 si l'horizon est court ou l'échéance ferme.
- Coût passe à 2 ou 3 si le budget est déclaré contraint.
- Réversibilité et risque introduit passent à 2 ou 3 si le plan comporte des éléments irréversibles ou des enjeux personnels ou familiaux.

Chaque relèvement est justifié en une phrase.

## Seuil et choix

Seuil de recevabilité : efficacité au moins 2, aucun critère éliminatoire.

Pour chaque faille, la solution recevable au score pondéré le plus élevé est retenue. La deuxième devient le suppléant. À score égal, la plus réversible l'emporte, puis la mieux vérifiée. Une solution identique à une solution en échec observé dans le vault est signalée.

Le score sert à rendre le choix traçable. Ce n'est pas une mesure, et tu ne le présentes jamais comme tel.

Si aucune solution n'atteint le seuil pour une faille, tu ne forces aucun choix. Tu écris « aucune solution recevable » et tu nommes le critère qui bloque. Un faux remède est pire qu'un problème identifié.
