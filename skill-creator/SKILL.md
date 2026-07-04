---
name: skill-creator
description: Construire ou améliorer un skill étape par étape, comme on apprend une tâche à quelqu'un. À déclencher quand l'utilisateur dit "crée un skill", "j'ai besoin d'un skill pour...", "améliore mon skill" ou "on construit un skill ensemble". On n'écrit jamais un skill d'un seul coup : chaque étape est produite, testée sur un cas réel, validée, puis versée dans le fichier avant de passer à la suite.
---

# skill-creator

Tu vas aider l'utilisateur à construire un skill fiable. Un skill fiable, ça se construit comme on apprend une tâche à quelqu'un : une étape à la fois, on regarde ce que ça donne sur un vrai cas, on corrige tout de suite, et on passe à la suite seulement quand c'est bon.

La tentation, c'est de générer un `SKILL.md` complet en une réponse. Ne le fais pas. Un skill généré à la volée prend à ta place toutes les décisions que l'utilisateur n'a pas prises : le format de sortie, les cas limites, les garde-fous. Ça produit un fichier propre à l'œil, bourré de choix par défaut invisibles, qui casse le jour où on l'utilise sur un vrai projet. La valeur d'un skill vient de la manière dont on le construit, pas du fichier lui-même.

## Règle du jeu

Pour chaque étape :

1. **Annonce** l'étape en une phrase : ce que tu vas faire et pourquoi.
2. **Produis** le contenu de l'étape. Dès que c'est possible, exécute-la pour de vrai sur le cas pilote et montre le résultat concret. Pas de "en théorie".
3. **Écris** ce qui est validé dans le fichier `SKILL.md` en construction. Le fichier grandit au fil des étapes.
4. **Attends** que l'utilisateur valide ou corrige avant de continuer. Jamais d'enchaînement automatique.

Si l'utilisateur corrige, la correction doit finir quelque part dans le skill :
- une information qui manquait devient un input requis ;
- une erreur qui revient devient un garde-fou ;
- un mauvais déclenchement devient un cas d'exclusion ;
- un bout de code réécrit à chaque fois devient un script.

C'est de là que vient l'adaptation : d'une vraie modification du fichier après un problème observé, pas d'une intention.

## La méthode en six étapes

### 1. Décrire le besoin en une phrase

Demande à l'utilisateur ce que le skill doit permettre de faire, en une ou deux phrases. N'exige pas un prompt parfait, c'est ton travail de le construire. Le but est de donner une direction. Exemple : « Un skill qui consolide mon rapport hebdo à partir de ces sources, dans ce format, sans inventer un seul chiffre. »

### 2. Poser les questions qui bloquent

Clarifie au minimum quatre choses : l'input (d'où viennent les données, sous quel format), le résultat attendu (à quoi ressemble une bonne sortie), les cas où le skill intervient, et les limites à respecter. Ne pose pas vingt questions d'un coup. Attaque ce qui bloque vraiment et démêle sujet par sujet. Reformule ce que tu comprends pour que l'utilisateur te corrige.

### 3. Choisir un vrai cas pilote

C'est l'étape que tout le monde saute, et c'est celle qui change tout. Demande une vraie demande concrète, avec de vraies données et un vrai résultat attendu. Le skill sera construit dessus, en remplissant la demande comme l'utilisateur le ferait à la main. Sans cas pilote, tu ne peux rien tester et tu restes dans l'abstrait. Si l'utilisateur n'en a pas, aide-le à en formuler un précis avant de continuer.

### 4. Définir la procédure

Propose les étapes nécessaires pour traiter le cas pilote. Pas trente. Si ta procédure dépasse sept étapes, le skill essaie probablement de faire trop de choses : propose de le scinder. Chaque étape doit avoir un input clair et un résultat observable. « Améliorer le rapport » n'est pas une étape. « Vérifier que chaque chiffre du message a une source associée » en est une : on peut la tester.

### 5. Exécuter et corriger une étape à la fois

C'est le cœur de la méthode. Pour chaque étape de la procédure : exécute-la sur le cas pilote, montre exactement ce que ça produit, écoute ce qui manque ou ne colle pas, mets à jour le fichier, puis passe à la suivante. Chaque correction est versée dans le skill avant de continuer.

### 6. Faire le dry run complet

Quand chaque étape passe séparément, relance toute la procédure depuis zéro sur un second cas pilote, différent du premier. Si le résultat est conforme, le skill est prêt. Si ça rate, identifie l'étape responsable, corrige, et relance la boucle. Un outil peut confirmer que le nom, le dossier et le fichier sont propres, mais ce qui compte c'est ce que le skill fait vraiment, et ça, seul l'utilisateur peut le juger sur son cas.

## Structure du fichier produit

Un skill est un dossier qui contient un fichier `SKILL.md`. Le `SKILL.md` commence par un frontmatter avec deux champs :

```yaml
---
name: nom-du-skill
description: Ce que fait le skill et quand le déclencher, en citant les tournures réelles que l'utilisateur emploierait.
---
```

La `description` est ce que l'IA lit pour décider d'activer le skill. Elle doit être précise sur le déclencheur, courte, sans jargon. « Un skill qui aide à faire X » ne déclenche rien.

Le corps contient les instructions : la procédure, les inputs requis, les garde-fous, le format de sortie. Ajoute des scripts, des exemples ou des références seulement quand une étape le justifie (un calcul fragile, un format à respecter à la lettre).

## Garde-fous

- Ne génère jamais le skill complet d'un seul coup.
- Ne saute jamais le cas pilote : sans sortie réelle à regarder, l'utilisateur ne peut pas corriger.
- Ne dépasse pas sept étapes dans la procédure du skill construit.
- Si la même étape est corrigée trois fois, demande à l'utilisateur de reformuler en ses mots ce qu'elle doit faire.
- N'invente pas d'input : si un dry run révèle qu'il manque une information, ajoute-la aux inputs requis.
- Le jugement reste chez l'utilisateur. Ton rôle est de verser ce jugement dans le fichier au lieu de le laisser partir en fumée à la fin de la conversation.
