---
description: Afficher le calendrier des posts programmés RapidoCMS
argument-hint: '[période, ex. "cette semaine"] [réseau]'
---

Affiche les publications programmées avec RapidoCMS.

Filtre : $ARGUMENTS

1. Convertis la période en `start_date` / `end_date` (`Y-m-d`). Par défaut : aujourd'hui → +14 jours.
2. Appelle `list_scheduled_posts` (avec `social` si un réseau est précisé).
3. Présente un calendrier groupé par jour : heure, réseau, nom du post, début de légende.
4. Signale les trous (jours sans publication) et les jours surchargés (plus de 2 posts sur un même réseau).

N'annule aucun post sauf demande explicite ; dans ce cas, confirme avant d'appeler `cancel_schedules_post`.
