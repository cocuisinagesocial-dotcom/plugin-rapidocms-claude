---
description: Statistiques des posts publiés via RapidoCMS
argument-hint: [période ou nom de post] [réseau]
---

Analyse les performances des publications RapidoCMS.

Filtre : $ARGUMENTS

1. Retrouve les posts concernés avec `list_scheduled_posts` (par période, nom ou réseau).
2. Appelle `post_insights` avec leurs ids (10 maximum par appel ; découpe si besoin).
3. Présente un tableau : post, réseau, date, portée, interactions, taux d'engagement (si les données le permettent).
4. Donne 3 enseignements concrets (meilleur réseau, meilleur format, meilleur créneau) et une recommandation pour les prochains posts.

N'invente aucun chiffre : si une métrique n'est pas renvoyée, indique « n/d ».
