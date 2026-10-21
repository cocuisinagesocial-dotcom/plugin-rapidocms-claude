# RapidoCMS Publisher — plugin Claude Code

Publier et programmer des posts Instagram, Facebook, LinkedIn et TikTok depuis Claude, en s'appuyant sur le serveur MCP **RapidoCMS**.

## Prérequis
Le connecteur **RapidoCMS** doit être connecté à votre compte Claude (claude.ai → Paramètres → Connecteurs), avec au moins un réseau social relié dans RapidoCMS.

## Installation
Depuis Claude Code :

```
/plugin marketplace add "C:/Users/amesk/OneDrive/Bureau/rapidocms plugin"
/plugin install rapidocms-publisher@rapidocms
```

## Contenu

| Élément | Rôle |
|---|---|
| Skill `publier-reseaux` | Workflow complet : contexte marque → comptes → média → rédaction par réseau → validation → brouillon → programmation → vérification. Se déclenche automatiquement quand vous demandez de publier. |
| `/publier <sujet> [réseaux] [date]` | Rédige et programme un post |
| `/brouillon <sujet> [réseaux]` | Crée seulement les brouillons |
| `/programmer-brouillon [nom/id] [date] [heure]` | Programme un brouillon existant |
| `/calendrier [période] [réseau]` | Affiche les posts programmés |
| `/stats [période/post] [réseau]` | Analyse les performances |
| Agent `community-manager` | Prépare un plan éditorial complet en brouillons |

## Exemples
- `/publier Nouveau menu d'automne, Instagram et Facebook, demain 18h30`
- `/brouillon 3 conseils pour réduire le gaspillage alimentaire, LinkedIn`
- `/calendrier cette semaine`
- « Prépare-moi une semaine de contenus pour le lancement de notre appli » → agent community-manager

## Sécurité
Aucun post n'est programmé sans un récapitulatif et votre validation explicite. Les identifiants de comptes, brouillons et URLs de médias viennent toujours de RapidoCMS, jamais inventés.
