# skill-creator

Un skill qui t'aide à construire tes skills. Pas en générant un fichier de 300 lignes en trente secondes, mais en te faisant avancer une étape à la fois, comme on apprend une tâche à quelqu'un.

Il marche avec **Claude Code, Codex, Gemini CLI et Google Antigravity** : le même fichier `SKILL.md`, sans modification. La procédure d'installation par outil est dans [INSTALL.md](INSTALL.md).

## Le problème qu'il règle

Quand tu génères un skill à partir d'une phrase vague, ton IA prend à ta place toutes les décisions que tu n'as pas prises : le format de sortie, les cas limites, les garde-fous. Tu récupères un fichier propre à l'œil, mais bourré de choix par défaut invisibles. Et ça ne se voit pas au premier coup d'œil. Ça te saute au visage le jour où tu l'utilises sur un vrai projet, et personne ne relit 300 lignes à froid pour comprendre pourquoi le résultat déraille.

Un skill fiable demande beaucoup plus de contexte et de décisions qu'un prompt normal. C'est un vrai système, avec ses règles et son format. La manière dont tu le construis compte plus que le fichier lui-même.

## La méthode

On construit un skill comme on apprend à faire un gâteau à un enfant : tu montres une étape, il la fait, tu regardes ce qui se passe, tu corriges maintenant. Tu n'attends pas que le gâteau sorte du four pour voir le problème.

Concrètement, six étapes :

1. **Décrire le besoin en une phrase.** Une direction suffit, pas un prompt parfait.
2. **Laisser le skill poser les questions.** Input, résultat attendu, cas d'usage, limites.
3. **Choisir un vrai cas pilote.** Une vraie demande, de vraies données. C'est l'étape que tout le monde saute, et celle qui change tout.
4. **Définir la procédure.** Des étapes observables, pas trente.
5. **Exécuter et corriger une étape à la fois.** Chaque correction est versée dans le fichier avant de passer à la suite.
6. **Faire le dry run complet.** On relance tout depuis zéro sur un second cas. Si ça passe, c'est prêt.

Le détail complet est dans le skill lui-même : [`skill-creator/SKILL.md`](skill-creator/SKILL.md).

## Utilisation

Une fois installé (voir [INSTALL.md](INSTALL.md)), appelle-le quand tu veux créer ou améliorer un skill :

```
Crée un skill pour consolider mon rapport hebdo à partir de Stripe, Analytics et MailerLite.
```

L'outil t'accompagne étape par étape. Tu n'as pas besoin de maîtriser la structure d'un `SKILL.md` avant de commencer. Tu dois juste pouvoir regarder son travail et lui dire : ça c'est bon ; ça ça ne correspond pas à ma manière de bosser.

## Structure du dépôt

```
skill-creator/
├── README.md            ce fichier
├── INSTALL.md           installation par outil (Claude, Codex, Gemini, Antigravity)
├── LICENSE
└── skill-creator/       le skill portable
    └── SKILL.md
```

## Licence

MIT. Prends-le, modifie-le, adapte-le à ta manière de bosser. Voir [LICENSE](LICENSE).

---

Construit par [Florian Brignoli](https://florianbrignoli.fr). Retours d'expérience sur les skills et les agents IA, une vidéo par semaine.
