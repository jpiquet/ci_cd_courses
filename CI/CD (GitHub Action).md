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