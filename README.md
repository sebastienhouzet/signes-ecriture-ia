# signes-ecriture-ia

Un skill pour que votre assistant IA écrive sans les tics qui trahissent un texte généré, et pour relire un texte existant à la recherche de ces mêmes tics.

Le contenu est tiré de la page [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing) (WP:AISIGNS), tenue par le WikiProject AI Cleanup. Cette page recense, exemples à l'appui, les tournures, le vocabulaire et les habitudes de mise en forme typiques des chatbots. Le skill la traduit en une checklist de 31 points, adaptée au français et à l'usage professionnel (mails, propositions commerciales, articles, posts).

## Contenu du dépôt

| Fichier | Rôle |
|---|---|
| `SKILL.md` | Le skill complet au format Agent Skills (frontmatter YAML + instructions). C'est le fichier à donner à Claude. |
| `prompt-complet.md` | Le même contenu sans le frontmatter, à coller dans les instructions d'un projet, d'un GPT, d'un Gem ou d'un agent. |
| `prompt-court.md` | Une version condensée (moins de 1 500 caractères) pour les champs « instructions personnalisées » à taille limitée. |
| `README.md` | Ce fichier. |
| `LICENSE` | CC BY-SA 4.0, héritée de Wikipedia. |

Le dépôt lui-même porte le nom du skill : cloné tel quel dans un dossier de skills, il fonctionne sans rien déplacer.

## Ce que fait le skill

Il a deux modes.

En mode écriture, l'assistant rédige normalement puis passe son texte au crible de la checklist avant de le livrer : vocabulaire IA (crucial, pivot, robuste, mettre en lumière…), analyses accrochées en fin de phrase par un participe présent (« …, soulignant ainsi… »), verbes qui remplacent « est » et « a » (constitue, représente, fait office de), parallélismes du type « non seulement… mais aussi », triplets systématiques, ton promotionnel, attributions vagues, tirets cadratins, gras à outrance, listes à en-tête gras, formules d'assistant laissées dans le texte. Chaque point vient avec sa correction. Le principe qui sous-tend tout : un LLM remplace le fait précis par la formule générique ; l'antidote est le concret.

En mode relecture, vous donnez un texte et l'assistant produit un rapport : chaque signe trouvé avec son numéro, la citation exacte et une proposition de correction, puis une appréciation en trois niveaux (artefacts certains, faisceau fort, signes isolés). Il rappelle les limites de l'exercice et ne conclut jamais « écrit par IA » sur le seul style, comme la page Wikipedia le recommande.

Le skill n'est pas un détecteur. C'est un guide de style en négatif, et un outil de relecture argumentée.

## Installation

Les chemins de menu et les limites de caractères ci-dessous sont ceux constatés en septembre 2026. Les éditeurs les changent souvent ; si un chemin ne correspond plus, cherchez « instructions personnalisées », « projet », « agent » ou « skill » dans les réglages de l'outil.

### Claude (application web, desktop, Cowork)

Claude lit nativement le format `SKILL.md`. Les skills personnalisés sont disponibles sur les offres Pro, Max, Team et Enterprise, avec l'option « exécution de code et création de fichiers » activée.

1. Téléchargez le dépôt (bouton Code puis Download ZIP sur GitHub) ou clonez-le.
2. Vérifiez que l'archive contient bien un dossier nommé `signes-ecriture-ia` avec `SKILL.md` à l'intérieur. Si GitHub a nommé le dossier `signes-ecriture-ia-main`, renommez-le avant de zipper : le nom du dossier doit être celui du skill.
3. Dans Claude, ouvrez Paramètres, puis Capacités (Settings > Capabilities), section Skills, et importez le fichier ZIP.
4. Le skill se déclenche seul dès que vous demandez de rédiger, réécrire ou relire un texte. Vous pouvez aussi le nommer : « applique signes-ecriture-ia à ce mail ».

Le skill est privé à votre compte. Sur Team ou Enterprise, un administrateur peut le déployer pour toute l'organisation.

### Claude Code

Un skill personnel se place dans `~/.claude/skills/`, un skill de projet dans `.claude/skills/` à la racine du dépôt concerné.

```bash
# pour tous vos projets
git clone https://github.com/<votre-compte>/signes-ecriture-ia ~/.claude/skills/signes-ecriture-ia

# pour un seul projet
git clone https://github.com/<votre-compte>/signes-ecriture-ia .claude/skills/signes-ecriture-ia
```

Claude Code détecte le skill au démarrage de la session suivante.

### Claude, via un projet plutôt qu'un skill

Si vous préférez ne rien importer, créez un projet (Projects > New Project), ouvrez ses réglages et collez le contenu de `prompt-complet.md` dans le champ Instructions. Le comportement sera le même à l'intérieur de ce projet.

### API Claude

Le format est le même : la documentation [Agent Skills](https://platform.claude.com/docs/en/agents-and-tools/agent-skills/overview) explique comment charger un dossier de skill dans une requête. À défaut, `prompt-complet.md` fonctionne comme system prompt.

### ChatGPT

Trois options, selon la place disponible.

Instructions personnalisées (icône de profil > Paramètres > Personnalisation > Instructions personnalisées). Le champ « Comment souhaitez-vous que ChatGPT réponde ? » est limité à 1 500 caractères en gratuit et 5 000 en payant. Collez `prompt-court.md` : il tient dans la limite gratuite. Les instructions s'appliquent aux nouvelles conversations.

Projet (Projects > New project > Instructions). Le champ est plus large : collez `prompt-complet.md`. Vous pouvez aussi joindre `SKILL.md` en fichier du projet et écrire dans les instructions « applique la checklist du fichier SKILL.md à tout texte que tu rédiges ou relis ».

GPT personnalisé (Explore GPTs > Create). Même principe : `prompt-complet.md` dans Instructions, ou `SKILL.md` en fichier de connaissances. Un GPT dédié est pratique si vous voulez un « relecteur » séparé de votre assistant habituel.

### Grok

Grok accepte des instructions personnalisées longues (12 000 caractères en 2026, la limite a varié dans l'année). Ouvrez le menu profil ou réglages en bas à gauche de grok.com, puis Custom Instructions, et collez `prompt-complet.md` en entier. Les Workspaces ont leurs propres instructions et leurs propres fichiers : un workspace « rédaction » avec `SKILL.md` en fichier joint et `prompt-complet.md` en instructions donne le meilleur résultat. Comme ailleurs, seules les nouvelles conversations prennent en compte le changement.

### Gemini

Créez un Gem (avatar en bas à gauche > Gems > Nouveau Gem). Donnez-lui un nom, collez `prompt-complet.md` dans Instructions, et ajoutez `SKILL.md` en fichier de connaissances si vous voulez qu'il puisse s'y référer mot pour mot. Le Gem apparaît ensuite dans la barre latérale.

### Mistral Le Chat

Menu Agents > Create Agent. Le champ Instructions est obligatoire : collez-y `prompt-complet.md`. Vous pouvez activer la recherche web ou l'interpréteur de code selon vos besoins, ce n'est pas nécessaire au skill.

### Microsoft 365 Copilot

Dans Copilot Chat : menu « … » en haut à droite > Paramètres de conversation > Personnalisation > Instructions personnalisées > Modifier. Le champ est court : utilisez `prompt-court.md`. Pour un Notebook Copilot, le même type de champ existe au niveau du notebook.

### Tout autre outil ou API

`prompt-complet.md` est un system prompt autonome. Il fonctionne tel quel avec n'importe quel modèle qui accepte des instructions système, y compris en local (Ollama, LM Studio, etc.).

## Utilisation

Quelques demandes qui déclenchent le skill :

```
Rédige un mail de relance à ce client, deux paragraphes, ton direct.
Réécris cette proposition commerciale sans jargon.
Relis ce texte : est-ce qu'il sent l'IA ? Où exactement ?
Applique signes-ecriture-ia à cet article et donne-moi la version corrigée.
```

En mode écriture, l'assistant ne commente pas la checklist : il livre un texte propre. En mode relecture, il cite chaque passage et propose une correction, puis donne une appréciation d'ensemble.

Si un texte livré vous paraît encore « sentir l'IA », demandez à l'assistant de reprendre la checklist point par point sur ce texte précis, plutôt que de lui demander une reformulation générale.

## Limites

La page Wikipedia le dit et le skill le répète : aucun de ces signes ne prouve, seul, qu'un texte a été généré. Les modèles sont entraînés sur de l'écriture humaine, les auteurs humains reproduisent certains de ces tics (surtout les non natifs ou les personnes formées à éviter les répétitions), et le langage courant s'aligne progressivement sur celui des LLM. Les études citées par la page donnent aux détecteurs automatiques comme au jugement humain des taux d'erreur qui interdisent d'accuser quelqu'un sur le style seul.

Les signes évoluent aussi avec les modèles. Le mot « delve » a quasiment disparu en 2025, les tirets cadratins reculent chez la plupart des éditeurs, et les formules sur la « couverture médiatique » et les sources progressent. Le skill note ces tendances, mais il faut le mettre à jour.

## Mettre à jour le skill

La page source change régulièrement. Pour régénérer la checklist, donnez la page à votre assistant et demandez-lui de comparer avec `SKILL.md` : quels signes ont été ajoutés, déplacés dans la section « Historical indicators », ou retirés. Reportez ensuite les changements dans les trois fichiers (`SKILL.md`, `prompt-complet.md`, `prompt-court.md`) pour qu'ils restent cohérents. Le script mental est simple : `prompt-complet.md` est `SKILL.md` sans son frontmatter, et `prompt-court.md` en est le résumé sous 1 500 caractères.

## Licence et attribution

[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.fr).