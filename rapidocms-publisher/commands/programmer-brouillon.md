---
description: Programmer un brouillon RapidoCMS existant
argument-hint: '[nom ou id du brouillon] [date] [heure]'
---

Programme un brouillon existant avec RapidoCMS.

Demande : $ARGUMENTS

1. Retrouve le brouillon avec `list_drafts_tool` (par `id` ou mot-clé `name`). S'il y a plusieurs résultats, liste-les et demande lequel.
2. Affiche son contenu (réseau, compte, légende, média) et la date/heure prévue (dates relatives converties en date absolue).
3. Pour TikTok, confirme `privacy_level` et les options (commentaires, duo, stitch, divulgation commerciale).
4. Attends la validation explicite, puis appelle `schedule_draft_tool`.
5. Vérifie avec `list_scheduled_posts` et confirme.
