# skill-creator

Le skill que j'utilise pour construire mes skills. Il t'empêche de générer un `SKILL.md` complet d'un coup et te fait avancer une étape à la fois, sur un vrai cas, en versant tes corrections dans le fichier au fur et à mesure.

Il marche avec **Claude Code, Codex, Gemini CLI et Google Antigravity** : ils lisent tous le même format `SKILL.md`, sans modification. L'installation par outil est dans [INSTALL.md](INSTALL.md).

## Le problème qu'il règle

Un skill fiable demande beaucoup plus de contexte et de décisions qu'un prompt normal : un format de sortie, des cas limites, des garde-fous. Quand tu le génères à partir d'une phrase vague, ton IA prend toutes ces décisions à ta place et te rend un fichier propre à l'œil, bourré de choix par défaut invisibles. Ça ne se voit pas tout de suite. Ça te saute au visage le jour où tu l'utilises sur un vrai projet, et personne ne relit 300 lignes à froid pour comprendre pourquoi ça déraille.

## Comment il fonctionne

Au lieu d'écrire le skill d'un bloc, il le construit sur un vrai cas pilote, une étape à la fois. Pour chaque étape : il propose l'action, l'exécute pour de vrai sur ton cas, te montre le résultat concret, prend ta correction, et met à jour le fichier avant de passer à la suite. Chaque correction finit quelque part dans le skill : une info manquante devient un input requis, une erreur récurrente devient un garde-fou, un bout de code réécrit à chaque fois devient un script. À la fin, il relance toute la procédure depuis zéro pour valider, puis installe le skill.

Le détail complet est dans le skill : [`skill-creator/SKILL.md`](skill-creator/SKILL.md).

## Utilisation

Une fois installé (voir [INSTALL.md](INSTALL.md)), appelle-le quand tu veux créer ou améliorer un skill :

```
Crée un skill pour consolider mon rapport hebdo à partir de Stripe, Analytics et MailerLite.
```

Il t'accompagne étape par étape. Tu n'as pas besoin de maîtriser la structure d'un `SKILL.md` avant de commencer. Tu dois juste pouvoir regarder son travail et lui dire : ça c'est bon ; ça ne correspond pas à ma manière de bosser.

## Structure du dépôt

```
skill-creator/
├── README.md            ce fichier
├── INSTALL.md           installation par outil (Claude, Codex, Gemini, Antigravity)
├── LICENSE              Apache 2.0
└── skill-creator/       le skill portable
    ├── SKILL.md         le workflow complet
    ├── scripts/         init_skill.py, quick_validate.py, generate_openai_yaml.py
    ├── references/      openai_yaml.md
    ├── agents/          openai.yaml (métadonnées UI, spécifiques Codex, optionnel)
    └── assets/          icônes
```

## Licence et origine

Ce skill est bâti sur le `skill-creator` open-source, sous licence Apache 2.0. Il est distribué sous la même licence, voir [LICENSE](LICENSE). Prends-le, modifie-le, adapte-le à ta manière de bosser.

---

Partagé par [Florian Brignoli](https://florianbrignoli.fr). Retours d'expérience sur les skills et les agents IA, une vidéo par semaine.
