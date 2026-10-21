---
name: publier-reseaux
description: Publier ou programmer un post sur Instagram, Facebook, LinkedIn ou TikTok via le connecteur MCP RapidoCMS. Utiliser dès que l'utilisateur veut publier, poster, programmer, planifier un post, préparer un brouillon social, ou décliner un contenu sur plusieurs réseaux avec RapidoCMS.
---

# Publier avec RapidoCMS

Ce skill s'appuie sur les outils du serveur MCP **RapidoCMS** (`https://cms.rapidosoftware.com/claude`), fourni par ce plugin sous le nom `rapidocms` (outils `mcp__plugin_rapidocms-publisher_rapidocms__create_draft_tool`, etc.). Si le connecteur claude.ai RapidoCMS est aussi actif, ses outils (`mcp__<id>__create_draft_tool`) sont équivalents : utilise ceux qui sont disponibles. Si aucun outil RapidoCMS n'est disponible ou si l'authentification est demandée, dis à l'utilisateur de lancer `/mcp`, de choisir `rapidocms` et de se connecter avec son compte RapidoCMS, puis arrête-toi.

## Outils utilisés

| Étape | Outil | Notes |
|---|---|---|
| Contexte marque | `get_company`, `get_brand` | Ton, valeurs, couleurs, cible |
| Comptes | `list_connected_accounts` | Donne le `page_id` à passer en `account_id` |
| Médiathèque | `list_all_files`, `upload_file_tool` | Le média doit avoir une URL publique |
| Visuel IA | `generate_image` | `size` = `hd` ou `standard` |
| Brouillon | `create_draft_tool`, `edit_draft_tool`, `list_drafts_tool` | |
| Programmation | `schedule_draft_tool` | Date `Y-m-d`, heure au format décrit par l'outil |
| Suivi | `list_scheduled_posts`, `cancel_schedules_post`, `post_insights` | |

## Déroulé

### 1. Comprendre la demande
Identifie : le sujet / message, le ou les réseaux, le média (fourni, à trouver dans la médiathèque, à générer, ou aucun), et le moment (maintenant, date/heure précise, ou simple brouillon). Ne pose de question que pour ce qui manque vraiment ; propose des valeurs par défaut sensées (ex. heure de forte audience : 12h30 ou 18h30).

### 2. Charger le contexte
- Appelle `get_company` (et `get_brand` si une marque est citée) pour respecter le ton et l'identité.
- Appelle `list_connected_accounts` (filtré sur les réseaux voulus). Si plusieurs comptes existent pour un même réseau, demande lequel utiliser. Si aucun compte n'est connecté pour un réseau demandé, signale-le et continue avec les autres.

### 3. Préparer le média
- **Média fourni par URL publique** : `upload_file_tool` (type `image` ou `video`) pour l'ajouter à la bibliothèque, puis utilise son URL.
- **Média de la bibliothèque** : `list_all_files` avec un mot-clé, propose les correspondances.
- **Visuel à créer** : `generate_image` avec un prompt précis reprenant l'identité de la marque ; montre le résultat avant de l'utiliser.
- Un fichier local de l'ordinateur ne peut pas être envoyé directement : demande une URL publique.

### 4. Rédiger pour chaque réseau
Rédige une légende **différente par réseau**, jamais un copier-coller :

| Réseau | Média | Style | Limite |
|---|---|---|---|
| Instagram | **obligatoire** (image ou vidéo) | accroche en 1re ligne, émojis modérés, 5–15 hashtags en fin | 2 200 caractères, 30 hashtags max |
| Facebook | optionnel | conversationnel, question ou CTA, 0–3 hashtags | court (< 500 conseillé) |
| LinkedIn | optionnel | professionnel, accroche forte, paragraphes courts, 3–5 hashtags | 3 000 caractères |
| TikTok | **vidéo obligatoire** | très court, punchy, 3–5 hashtags tendance | 2 200 caractères |

`post_type` : `text` (sans média), `media` (média seul), `mediatext` (média + texte — le cas le plus courant).

### 5. Valider avec l'utilisateur — OBLIGATOIRE
Publier est une action publique et irréversible. Avant tout `schedule_draft_tool`, affiche un récapitulatif par réseau :

```
📱 Instagram — @compte
🖼️ Média : <url>
📝 Légende : <texte complet>
🗓️ Programmation : 2026-10-21 à 18:30
```

et attends un « oui » explicite. Si l'utilisateur demande des modifications, ajuste et re-présente. La création d'un simple brouillon (`create_draft_tool`) peut se faire sans confirmation, mais signale-la.

### 6. Créer les brouillons
Un `create_draft_tool` par réseau :
- `post_name` : nom court et explicite (ex. `Lancement menu automne – IG`)
- `social_type` : `instagram` | `facebook` | `linkedin` | `tiktok`
- `account_id` : le `page_id` du compte
- `post_type`, `media_type` (`image`/`video`), `media_url`, `media_caption`
- `media_source` : toujours `biblio`
- Pour un post texte, passe `media_type` et `media_url` vides.

Conserve le `draft_id` renvoyé.

### 7. Programmer / publier
`schedule_draft_tool` avec `draft_id`, `post_date` (`Y-m-d`) et `post_heure` (respecte strictement le format indiqué dans la description de l'outil ; en cas d'erreur de format, réessaie avec `HH:MM:SS`).
- « Publier maintenant » : programme à l'heure actuelle + 5 minutes.
- **TikTok** : demande ou confirme `privacy_level` (par défaut `SELF_ONLY` — préviens que la vidéo sera privée ; `PUBLIC_TO_EVERYONE` pour public), `disable_comment`, `disable_duet`, `disable_stitch`, et la divulgation commerciale (`your_brand` = promotion de sa propre activité, `content_brand` = partenariat rémunéré).

### 8. Confirmer
Termine par un tableau : réseau, compte, nom du brouillon, date/heure, statut. Vérifie avec `list_scheduled_posts` que les posts apparaissent. Si un appel échoue, donne l'erreur réelle et ne prétends pas que c'est publié.

## Règles
- Ne jamais publier ni programmer sans validation explicite de l'utilisateur dans la conversation.
- Ne jamais inventer un `account_id`, un `draft_id` ou une URL de média : ils viennent toujours des outils.
- Annuler un post programmé (`cancel_schedules_post`) uniquement à la demande de l'utilisateur, après confirmation.
- Les dates relatives (« demain », « vendredi ») sont converties en dates absolues et affichées dans le récapitulatif.
