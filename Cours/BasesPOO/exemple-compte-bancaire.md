# Un exemple : le compte bancaire

Supposons qu'on veuille développer un compte bancaire. En programmation procédurale, sans objet, on pourrait écrire par exemple le code C suivant pour gérer un compte (retrait, dépôt) :

```c
/* fichier: compte.c - Style procédural PROBLÉMATIQUE */

int solde = 1000;

void retirer(int montant) {
    if (solde >= montant) {
      solde -= montant;
    }
}

void deposer(int montant) {
    solde += montant;
}
```

Les fonctions `retirer` et `deposer` que nous avons définies gèrent l'accès au solde du compte. En particulier, la fonction `retirer` vérifie que le solde est suffisant pour le retrait. Le problème est que la variable `solde` n'est pas protégée, c'est à dire que depuis un autre fichier, on pourrait facilement la modifier sans passer par les fonctions. Par exemple :
```c
/* Dans n'importe quel autre fichier du projet : */
extern float solde;  /* Accessible partout ! */

void fonction_malveillante() {
    solde = -5000.0;  /* CATASTROPHE : personne ne peut l'empêcher */
}
```

## Résumons les problèmes
- N'importe quelle partie du programme peut accéder et modifier `solde_compte`
- Les données sont complètement exposées en lecture et surtout en écriture
- Aucun moyen de garantir la cohérence des données

Les données ne sont pas *"protégées"*, un programmeur mal intentionné ou plus probablement un programmeur peu averti peu facilement les modifier et altérer leur cohérence. Dans le cas de notre compte, l'accès à la variable solde n'est pas protégé, un programmeur pourrait modifier le solde directement.