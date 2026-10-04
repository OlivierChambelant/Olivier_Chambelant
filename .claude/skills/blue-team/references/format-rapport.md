# Format du rapport Blue Team

Le rapport suit exactement cette structure.

1. SYNTHÈSE : une phrase. Nombre de failles traitées, de solutions retenues, de failles restées ouvertes, de failles reportées.
2. ÉQUIPE : une ligne par sous-agent, profil et spécialité. Inclut l'ingénieur de faisabilité et les deux attaquants de la contre-passe.
3. PÉRIMÈTRE : failles traitées, failles reportées au-delà du plafond, failles non traitées et pourquoi.
4. PONDÉRATIONS : critère, poids, raison de chaque relèvement.
5. SOLUTIONS PAR FAILLE, dans l'ordre de gravité du rapport Red Team, avec une sous-section par option si plusieurs options sont traitées. Pour chaque faille :
   - Faille : identifiant et rappel en une ligne.
   - Retenue : profil, solution, point d'action, coût, délai, réversibilité, prérequis, statut de vérification, niveau de confiance qualitatif et ce qui le fait varier.
   - Pourquoi : les critères qui l'ont fait gagner, en deux phrases maximum.
   - Écartées : une ligne par proposition, avec le critère éliminatoire ou l'écart de score qui l'élimine.
   - Noyau : ce que l'extraction a tiré des profils CRÉATIF et WTF, et ce qu'il est devenu.
   - Suppléant : profil et solution en une ligne.
6. CONTRE-PASSE : failles trouvées, substitutions effectuées, conflits résolus.
7. FAILLES OUVERTES : celles sans solution recevable, avec le critère bloquant.
8. DONNÉES VÉRIFIÉES : tableau. Fait, valeur, source, statut, date de consultation.
9. HISTORISATION : fiches Blue créées (titre et chemin), fiches Red mises à jour, lignes d'index modifiées, fichiers de `runs/`. Chaque élément porte « enregistrée » seulement après relecture réussie. Sans accès au vault, les fiches et lignes d'index en blocs markdown sous le titre « À coller dans Coffre-fort », et le chemin local des fichiers de runs.

Rien après le point 9.

## Exemple positif, pour une faille

« F1, fatale. Le permis de travail risque d'arriver après la date de prise de poste, l'offre tombe.
Retenue : PRAGMATIQUE. Négocier une date de début conditionnée à la délivrance du permis, inscrite dans la lettre d'offre. Point d'action : mécanisme. Coût : aucun coût direct, une négociation (estimation). Délai : immédiat. Réversibilité : totale. Prérequis : une offre écrite. Statut : aucun fait daté. Confiance : moyenne, dépend de la souplesse de l'employeur. À valider par un professionnel réglementé : consultant réglementé en immigration ou avocat.
Pourquoi : elle casse le mécanisme lui-même, puisque l'offre ne dépend plus du délai. Meilleur score sur l'efficacité et la réversibilité, deux critères relevés par l'enjeu familial.
Écartées : ÉPROUVÉ, marge de sécurité dans le calendrier, efficacité 1 car elle réduit l'impact sans toucher au mécanisme. SYSTÉMIQUE, refonte de la séquence, délai incompatible avec l'horizon. WTF, obtenir le statut avant de chercher l'employeur, éliminatoire car brise la contrainte déclarée de disponibilité sous 3 mois.
Noyau : du WTF, lancer dès maintenant toutes les démarches qui ne dépendent pas de l'employeur, pour raccourcir la chaîne après l'offre. Retenu comme suppléant.
Suppléant : NOYAU WTF, démarches indépendantes de l'employeur lancées avant l'offre. »

## Exemple négatif

« Il faudrait mieux anticiper les délais et prévoir un plan B en cas de retard. »

Ce format échoue : aucun point d'action sur la faille, aucun coût, aucun délai, aucune alternative comparée, aucune raison de rejet. L'utilisateur ne peut ni appliquer la solution ni contester le choix.
