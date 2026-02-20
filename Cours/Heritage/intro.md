# L'héritage en programmation orientée objet

## Les origines : Simula 67 (1967)

L'héritage est né d'un besoin concret de réutilisation de code dans les simulations. En 1967, Ole-Johan Dahl et Kristen Nygaard ont présenté un concept permettant de spécifier des objets appartenant à différentes classes mais ayant des propriétés communes. Ces propriétés communes étaient regroupées dans une superclasse, et chaque superclasse pouvait elle-même avoir une superclasse (voir [la page Wikipedia dédiée](https://en.wikipedia.org/wiki/Simula) ou un [historique du langage Simula](https://users.dcc.uchile.cl/~cgutierr/cursos/LP/SimulaHistory.html) écrit par Jan Rune Holmevik).

## La raison d'être

L'**héritage** est un mécanisme qui permet à une classe (appelée **sous-classe** ou **classe dérivée**) de récupérer automatiquement les attributs et méthodes d'une autre classe (appelée **superclasse** ou **classe de base**).

Si la classe B hérite de la classe A, alors on peut écrire que **`B est un A`**. Par exemple :
* Mammifère **est un** type d'animal
* Chien **est un** type de mammifère
* Chat **est un** type de mammifère

Ces relations **est un** entre les classes permettent d'exprimer une **hiérarchie de classe**, c'est à dire un arbre dans lequel les noeuds sont les classes et les arcs orientés la relation **est un**. On utilise les diagrammes de classes UML pour représenter cela :

```mermaid
classDiagram
    class Animal {
    }
    class Mammifère {
    }
    class Chien {
    }
    class Chat {
    }


    Animal <|-- Mammifère
    Mammifère <|-- Chien
    Mammifère <|-- Chat

```


L'héritage permet aux programmeurs de créer des classes qui sont construites sur des classes existantes, de spécifier une nouvelle implémentation tout en maintenant les mêmes comportements, de réutiliser du code et d'étendre de manière indépendante un logiciel via des classes et interfaces publiques.

Reprenons l'exemple des comptes bancaires. Supposons qu'il existe différents types de comptes :

* Le compte courant : Celui sur lequel on retire de l'argent, qui peut avoir un découvert autorisé.
* Le compte d'épargne : Un compte qui n'autorise pas de découvert, sur lequel on va placer de l'argent qui sera rémunéré par un taux d'intérêt.
* Le compte professionnel : à destination d'une entreprise, il référence la raison social de cette entreprise.

Tous ces comptes partagent aussi des informations communes :

* Une indication de leur titulaire
* Un numéro de compte
* Un solde

En plus de cela, on aimerait pouvoir faire des opérations sur tous ces comptes :

* Retirer une somme d'argent
* Déposer une somme d'argent
* consulter le solde


On cherche à représenter ces informations de sorte à faire apparaitre et à pouvoir exploiter les relation de type **est un**. Par exemple qu'un compte courant **est un** type de compte. On va utiliser pour cela la notion d'héritage que nous illustrons avec le diagramme de classe UML suivant :

```mermaid
classDiagram
    class Compte {
        -titulaire: String
        -numero: String
        -solde: int
        +deposer(m: int)
        +retirer(m: int)
        +obtenirSolde()
    }
    
    class CompteCourant {
        -decouvertAutorise: int
    }
    
    class CompteEpargne {
        -tauxInteret: int
    }
    
    class CompteProfessionnel {
        -raisonSociale: String
    }
    
    Compte <|-- CompteCourant
    Compte <|-- CompteEpargne
    Compte <|-- CompteProfessionnel
```

<div style="text-align: center;"><i>Hiérarchie de comptes bancaires</i></div>



<hr/><hr/><hr/><hr/><hr/><hr/>

## Les bénéfices de l'héritage

1. **Réutilisation du code** : Pas besoin de réécrire `deposer()`, `afficherInfos()`, etc. pour chaque type de compte.

2. **Extension facile** : Pour ajouter un nouveau type de compte (par exemple, `CompteJeune`), il suffit d'étendre `Compte` et de redéfinir les méthodes spécifiques.

3. **Maintenance simplifiée** : Si on doit changer la logique de `deposer()`, on ne la modifie qu'à un seul endroit : dans la classe `Compte`.

4. **Polymorphisme** : On peut traiter tous les comptes de manière uniforme via le type `Compte`, tout en conservant leurs comportements spécifiques.

## Les critiques modernes de l'héritage

### Le problème de la "classe de base fragile" (Fragile Base Class Problem)

Le principal problème avec l'héritage d'implémentation est qu'il introduit un couplage non nécessaire sous la forme du "problème de la classe de base fragile" : des modifications à l'implémentation de la classe de base peuvent causer des changements de comportement involontaires dans les sous-classes [3].

```mermaid
sequenceDiagram
    participant Client
    participant Stack
    participant ArrayList
    
    Note over ArrayList: Modification de la classe de base
    Note over ArrayList: addAll() appelle add() en interne
    
    Client->>Stack: addAll([1,2,3])
    Stack->>ArrayList: addAll([1,2,3])
    Note over ArrayList: Hérite de ArrayList !
    ArrayList-->>Stack: Ajout multiple
    Note over Stack: ❌ Comportement compromis<br/>Une pile ne devrait pas avoir addAll()
```

*Schéma 5 : Le problème de la classe de base fragile - modifications involontaires*

**Exemple du problème :**

```java
// Classe de base - Version 1
public class Stack<E> extends ArrayList<E> {
    public void push(E element) {
        add(element);
    }
    
    public E pop() {
        return remove(size() - 1);
    }
}

// On utilise Stack dans tout le projet...

// Quelqu'un modifie ArrayList (classe de base) :
// ArrayList ajoute une nouvelle méthode addAll() qui appelle add() en interne

// Maintenant, si quelqu'un fait :
Stack<Integer> stack = new Stack<>();
stack.addAll(Arrays.asList(1, 2, 3));  // Hérite de ArrayList !

// Le comportement de Stack est compromis car addAll() 
// n'est pas une opération valide pour une pile !
```

### La recommandation moderne : "Composition over Inheritance"

Les auteurs du livre Design Patterns recommandent plutôt l'héritage d'interface et favorisent la composition plutôt que l'héritage [4].

```mermaid
graph LR
    subgraph "❌ Héritage (couplage fort)"
        Stack1[Stack] -->|extends| ArrayList1[ArrayList]
        Note1[Stack hérite de TOUT<br/>y compris addAll, clear, etc.]
    end
    
    subgraph "✅ Composition (couplage faible)"
        Stack2[Stack] -->|contient| ArrayList2[ArrayList]
        Note2[Stack expose seulement<br/>push et pop]
    end
    
    style Stack1 fill:#e74c3c
    style ArrayList1 fill:#e74c3c
    style Stack2 fill:#27ae60
    style ArrayList2 fill:#27ae60
```

*Schéma 6 : Héritage vs Composition - la composition offre un meilleur contrôle*

**Alternative avec composition :**

```java
// Au lieu d'hériter, on CONTIENT une ArrayList
public class Stack<E> {
    private ArrayList<E> elements = new ArrayList<>();  // Composition !
    
    public void push(E element) {
        elements.add(element);
    }
    
    public E pop() {
        return elements.remove(elements.size() - 1);
    }
    
    public int size() {
        return elements.size();
    }
    
    // On expose UNIQUEMENT les méthodes qu'on veut !
    // addAll() n'est PAS accessible de l'extérieur
}
```

### James Gosling et Java

Il a été rapporté que James Gosling, l'inventeur de Java, s'est exprimé contre l'héritage d'implémentation, déclarant qu'il ne l'inclurait pas s'il devait reconcevoir Java [5].

## Quand utiliser l'héritage ?

```mermaid
graph TD
    A[Besoin de réutiliser du code?] -->|Oui| B{Relation entre classes?}
    B -->|est-un| C{Stable et claire?}
    B -->|a-un ou utilise-un| D[❌ Composition]
    C -->|Oui| E{Hiérarchie simple?}
    C -->|Non| D
    E -->|Oui 2-3 niveaux| F[✅ Héritage OK]
    E -->|Non complexe| D
    A -->|Non| G[Pas besoin d'héritage]
    
    style F fill:#27ae60,color:#fff
    style D fill:#3498db,color:#fff
    style G fill:#95a5a6,color:#fff
```

*Schéma 7 : Arbre de décision - héritage ou composition ?*

### ✅ Utiliser l'héritage quand :

1. **Relation "est-un" claire et stable**
   - Un Chien **est un** Animal (ne changera jamais)
   - Une ArrayList **est une** List (relation fondamentale)

2. **Hiérarchie simple et peu profonde**
   - 2-3 niveaux maximum
   - Pas de modifications fréquentes de la classe de base

3. **Polymorphisme nécessaire**
   - Besoin de traiter des objets différents de manière uniforme
   - Frameworks et APIs publiques

### ❌ Éviter l'héritage quand :

1. **Relation "a-un" ou "utilise-un"**
   - Une Voiture **a un** Moteur → Composition !
   - Un Stack **utilise une** ArrayList → Composition !

2. **Besoin de flexibilité**
   - Changement de comportement à l'exécution
   - Combinaison de fonctionnalités multiples

3. **Risque de fragile base class**
   - Classe de base susceptible de changer
   - Hiérarchie complexe

## Conclusion

L'héritage est un outil puissant inventé dans les années 1960 pour résoudre le problème de la duplication de code dans les systèmes de simulation. Simula 67 a introduit tous les éléments aujourd'hui considérés comme essentiels dans un langage orienté objet : encapsulation, héritage, liaison tardive, et création dynamique d'objets [1].

Cependant, l'expérience de 50+ années de programmation orientée objet a montré que l'héritage doit être utilisé avec précaution :
- **Excellent** pour modéliser des relations "est-un" stables et claires
- **Problématique** quand utilisé simplement pour réutiliser du code
- **Souvent remplaçable** par la composition, qui offre plus de flexibilité

La règle moderne : **"Favoriser la composition plutôt que l'héritage, sauf quand la relation 'est-un' est évidente et stable"**.

---

## Références

[1] **Wikipedia - Simula**  
"Class and inheritance concepts - Simula 67"  
https://en.wikipedia.org/wiki/Simula

[2] **Wikipedia - Inheritance (object-oriented programming)**  
"Definition and fundamental principle of inheritance"  
https://en.wikipedia.org/wiki/Inheritance_(object-oriented_programming)

[3] **Wikipedia - Fragile base class**  
"The fragile base class problem in implementation inheritance"  
https://en.wikipedia.org/wiki/Fragile_base_class

[4] **Design Patterns: Elements of Reusable Object-Oriented Software**  
Gamma, E., Helm, R., Johnson, R., & Vlissides, J. (1994)  
"Favor object composition over class inheritance"

[5] **James Gosling on Java**  
Diverses interviews et conférences où James Gosling a exprimé ses réserves sur l'héritage d'implémentation

---

*Document créé pour l'enseignement de la programmation orientée objet - Février 2026*