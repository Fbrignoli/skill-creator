# Installation

Le skill est un dossier `skill-creator/` qui contient un `SKILL.md`. L'installer, c'est copier ce dossier là où ton outil va chercher ses skills. Le même fichier marche partout, sans modification.

Récupère le dépôt d'abord :

```bash
git clone https://github.com/Fbrignoli/skill-creator.git
cd skill-creator
```

Ensuite, choisis ton outil.

## Le chemin universel : `.agents/skills/`

Claude Code, Gemini CLI et Antigravity reconnaissent tous le dossier `.agents/skills/` à la racine d'un projet. Si tu veux le skill dans un projet précis, un seul geste suffit pour ces trois outils :

```bash
mkdir -p .agents/skills
cp -r skill-creator .agents/skills/
```

Pour une installation globale (dispo dans tous tes projets), suis la section de ton outil ci-dessous.

## Claude Code

**Global** (tous tes projets) :

```bash
mkdir -p ~/.claude/skills
cp -r skill-creator ~/.claude/skills/
```

**Un seul projet** :

```bash
mkdir -p .claude/skills
cp -r skill-creator .claude/skills/
```

Relance Claude Code. Le skill se déclenche tout seul quand tu demandes de créer ou d'améliorer un skill. Vérifie avec `/skills` s'il apparaît dans la liste.

## Codex

```bash
mkdir -p ~/.codex/skills
cp -r skill-creator ~/.codex/skills/
```

Le skill est chargé au prochain lancement. Tu peux l'appeler directement : « utilise skill-creator pour... ».

## Gemini CLI

**Global** :

```bash
mkdir -p ~/.gemini/skills
cp -r skill-creator ~/.gemini/skills/
```

**Un seul projet** : utilise `.agents/skills/` (voir plus haut), c'est le chemin recommandé par Gemini pour rester compatible entre outils.

## Google Antigravity

**Global** (reconnu par toutes les variantes d'Antigravity) :

```bash
mkdir -p ~/.gemini/config/skills
cp -r skill-creator ~/.gemini/config/skills/
```

**Un seul projet** : utilise `.agents/skills/` à la racine du workspace (voir plus haut).

## Vérifier que ça marche

Peu importe l'outil, le vrai test n'est pas que le fichier soit au bon endroit. C'est ce qu'il fait. Lance :

```
Crée un skill pour <un vrai besoin à toi, en une phrase>.
```

Si l'outil commence à te poser des questions au lieu de cracher un fichier complet d'un coup, le skill est actif et fait son travail.

## Mettre à jour

Le skill est fait pour être modifié. Ouvre `SKILL.md`, change ce qui ne colle pas à ta manière de bosser, et recopie le dossier au même endroit. C'est ton fichier maintenant.
