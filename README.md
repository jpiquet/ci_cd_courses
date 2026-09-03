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

### *Vérifier les actions avant de les utiliser*

Avant d'ajouter une nouvelle action, demandez-vous :

- Qui l'a créée ? (GitHub, entreprise connue, inconnu ?)
- Est-elle maintenue ? (dernière mise à jour récente ?)
- Combien de personnes l'utilisent ? (populaire = plus d'yeux dessus)
- Ai-je vraiment besoin d'une action ? (parfois un run: suffit)

