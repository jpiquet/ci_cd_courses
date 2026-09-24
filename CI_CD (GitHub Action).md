# ci_cd_courses
A repository to learn basics of CI/CD &amp; train

## Les steps:

**RUN** execute une commande shell.
**USES** utilise une action (c'est un bloc de code reutilisable).

## Sécurité

Les workflows GitHub action on accés au secrets, peuvent modifier le code et déployer du code malveillant. Voila pourquoi sécuriser son workflow est très important.

### SHA

Un SHA (Secure Hash Algorithm) est l'empreinte cryptographique d'un commit. Contrairement à un tag, il est immuable, impossible de le modifier sans changer le hash.

```
# ❌ DANGEREUX : tag mutable
- uses: actions/checkout@v4

# ✅ SÉCURISÉ : SHA épinglé
- uses: actions/checkout@b4ffde65f46336ab88eb53be808477a3936bae11 # v4.1.1
```
Comment trouver le SHA ?

```bash
# Via l'API GitHub
curl -s https://api.github.com/repos/actions/checkout/commits/v4 | jq -r .sha

# Via la CLI gh
gh api repos/actions/checkout/commits/v4 --jq .sha
```

On peut soit trouver manuellement le SHA sur GitHub, soit faire ca automatiquement avec **pin-github-action**.

```bash
npx pin-github-action .github/workflows/ci.yml
```

### *Vérifier les actions avant de les utiliser*

Avant d'ajouter une nouvelle action, demandez-vous :

- Qui l'a créée ? (GitHub, entreprise connue, inconnu ?)
- Est-elle maintenue ? (dernière mise à jour récente ?)
- Combien de personnes l'utilisent ? (populaire = plus d'yeux dessus)
- Ai-je vraiment besoin d'une action ? (parfois un run: suffit)

#### *Récapitulatif : checklist de base*

Avant de mettre un workflow en production :

##### Secrets et permissions :

- Pas de secrets dans le code, utilisez ${{ secrets.* }}
- Pas d'injection, données externes passées via env:, jamais interpolées directement dans run:
- Permissions déclarées, permissions: en haut du workflow avec le minimum nécessaire
- Pas de permissions: write-all, listez uniquement ce dont vous avez besoin

##### Actions et dépendances :

- Actions vérifiées, de source connue et maintenue
- Orthographe vérifiée, pas de typosquatting (actions/checkout et non action/checkout)
- Évaluer les dépendances, utilisez OpenSSF Scorecard pour vérifier la maturité sécurité des projets
- Actions épinglées par SHA, pas de @v1 ou @latest

##### Pull requests :

-Pas de pull_request_target, sauf si vous comprenez les risques
-Approbation requise, pour les workflows sur PR de contributeurs externes

##### Bonnes pratiques avancées :

- Dependabot activé, pour les mises à jour automatiques des actions
- Branch protection, exiger des reviews et des checks avant merge
- Audit régulier, vérifier périodiquement les actions utilisées


## Secrets & Variables :

Les secrets doivent être stocké dans dans les **Secrets de GitHub**.
Ils ne doivent en aucun cas être écrit en dur dans un .yaml par exemple.
Il y a différent niveau d'accessibilité des secrets dans GitHub:

| Niveau       |	Portée	                 | Cas d'usage                               |
| -------------| --------------------------  |-------------------------------------------|
| Organisation |	Tous les repos de l'org	 | Token Docker Hub partagé par l'équipe     |
| Repository   |	Un seul repo	         | Clé de déploiement spécifique au projet   |
| Environment  |  Un environnement du repo   | Credentials de la base de prod vs staging |

## GitHub Marketplace

Le **marketplace** de Github regroupe des millers d'action réutilisable créees par Github, des éditeurs et a communauté.

Un action est un composant réutilisable qui encapsule une tâche.
Au lieux d'ecrire des lignes shell on va utilisé une action qui existe déjà qui va effectué par exemple:
- Téléchargement
- Installation
- Configuration du PATH
- Mise en caches des dépendences.
D'un service qu'on voudrait tester.

#### Référence d'action:

Quand on écrit `action/checkout@v4` après `uses:`, voici comment c'est décomposé:

```yaml
uses: actions/checkout@3d3c42e5aac5ba805825da76410c181273ba90b1  # v7.0.1
       └──────┬──────┘ └┬┘
          owner/repo   ref
```

- owner : L'organisation ou l'utilisateur GitHub (ici actions, l'org officielle GitHub)
- repo : Le nom du repository contenant l'action
- ref : La version à utiliser (tag, branche, ou SHA)

### Sécurité : les bons réflexes
Utiliser une action tierce, c'est exécuter du code d'un inconnu avec accès à vos secrets. Adoptez ces réflexes dès maintenant :

#### 1. Épingler par SHA

Au lieu des tags mutables, utilisez le SHA complet du commit :

```bash
##### ❌ Tag mutable - peut changer sans prévenir
- uses: actions/checkout@v4

##### ✅ SHA immuable - vous contrôlez exactement le code exécuté
- uses: actions/checkout@11bd71901bbe5b1630ceea73d27597364c9af683 # v4.2.2
```
Le commentaire # v4.2.2 conserve la lisibilité tout en garantissant l'immutabilité.

Comment trouver le SHA ?

```bash
# Via l'API GitHub
curl -s https://api.github.com/repos/actions/checkout/commits/v4 | jq -r .sha

# Via la CLI gh
gh api repos/actions/checkout/commits/v4 --jq .sha
```
