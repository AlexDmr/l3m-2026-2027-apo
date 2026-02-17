# Pourquoi la programmation orientée objet ?

## Le contexte historique : un problème de complexité

Dans les années 1960, les programmeurs faisaient face à un problème majeur : la gestion et la maintenance du code devenaient un cauchemar à mesure que les logiciels se complexifiaient. Avec la programmation procédurale (Fortran, COBOL, C), les données et les fonctions étaient séparées, ce qui signifiait que les changements dans une partie du code pouvaient perturber une autre partie.

C'est dans ce contexte qu'en Norvège, deux chercheurs, Ole-Johan Dahl et Kristen Nygaard, créent Simula entre 1961 et 1967. Leur objectif initial était de simuler le mouvement de navires dans des ports. Ce langage a introduit des mécanismes fondamentalement nouveaux : le concept de classe pour l'encapsulation de la structure des données et du code, les sous-classes pour la réutilisation du code, et la création dynamique d'objets.

## Quels sont les problèmes sans l'encapsulation ?

L'encapsulation est **le concept central** de la programmation objet.

### L'analogie d'Alan Kay

Alan Kay, qui a popularisé le terme "programmation orientée objet" en 1966-1967, pensait aux objets comme à des cellules biologiques ou des ordinateurs individuels sur un réseau, capables uniquement de communiquer par messages. Dans une interview ultérieure, Kay a précisé que pour lui, la POO signifiait "seulement la messagerie, la rétention et protection locale de l'état, et le masquage de l'état-processus".









## Conclusion sur l'encapsulation
L'encapsulation n'est pas qu'un détail technique : c'est une philosophie de conception qui dit "regroupons les données avec les opérations qui les manipulent, protégeons l'état interne, et communiquons par interfaces bien définies". 

En C procédural, les données globales et les fonctions sont séparées, exposées, vulnérables. En Java (et dans les langages objets en général), les mécanismes d'encapsulation sont directement intégrés au langage avec les modificateurs d'accès (`private`, `public`, `protected`).