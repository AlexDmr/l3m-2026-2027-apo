# Pourquoi la programmation orientée objet ?

## Le contexte historique : un problème de complexité

Dans les années 1960, les programmeurs faisaient face à un problème majeur : la gestion et la maintenance du code devenaient un cauchemar à mesure que les logiciels se complexifiaient. Avec la programmation procédurale (Fortran, COBOL, C), les données et les fonctions étaient séparées, ce qui signifiait que les changements dans une partie du code pouvaient perturber une autre partie.

C'est dans ce contexte qu'en Norvège, deux chercheurs, Ole-Johan Dahl et Kristen Nygaard, créent Simula entre 1961 et 1967. Leur objectif initial était de simuler le mouvement de navires dans des ports. Ce langage a introduit des mécanismes fondamentalement nouveaux : le concept de classe pour l'encapsulation de la structure des données et du code, les sous-classes pour la réutilisation du code, et la création dynamique d'objets.

## Quels sont les problèmes sans l'encapsulation ?

L'encapsulation est **le concept central** de la programmation objet.

### L'analogie d'Alan Kay

Alan Kay, qui a popularisé le terme "programmation orientée objet" en 1966-1967, pensait aux objets comme à des cellules biologiques ou des ordinateurs individuels sur un réseau, capables uniquement de communiquer par messages. Dans une interview ultérieure, Kay a précisé que pour lui, la POO signifiait "seulement la messagerie, la rétention et protection locale de l'état, et le masquage de l'état-processus".




## En Java ?

Pour vous donner une idée, voilà comment on pourrait écrire en Java une classe Compte et comment on pourrait l'instancier et l'utiliser. On verra de façon plus précise dans la suite de ce cours toutes ces notions.

```java
public class Compte {
    private int solde; // On déclare l'entier solde comme attribue privé, non visiable à l'extérieur
    
    // Constructeur d'objet
    public Compte(int soldeInitial) {
        this.solde = soldeInitial;
    }
    
    // Méthodes publiques
    public void retirer(int somme) {
        if (somme <= this.solde) {
            this.solde -= somme;
        }
    }
    
    public void deposer(int somme) {
        if (somme > 0) {
            this.solde += somme;
        }
    }
    
    public int obtenirSolde() {
        return this.solde;
    }
    
    // Méthode main pour tester
    public static void main(String[] args) {
        Compte monCompte = new Compte(1500);

        monCompte.solde = 30; // Cette ligne ne compile pas, accès direct impossible à solde

        monCompte.deposer(500);   // On dépose 500, on a 2000
        monCompte.retirer(300);   // On retire 300, on a 1700
        monCompte.retirer(2000);  // Tentative de retrait supérieur au solde
    }
}
```




## Conclusion sur l'encapsulation
L'encapsulation n'est pas qu'un détail technique : c'est une philosophie de conception qui dit "regroupons les données avec les opérations qui les manipulent, protégeons l'état interne, et communiquons par interfaces bien définies". 

En C procédural, les données globales et les fonctions sont séparées, exposées, vulnérables. En Java (et dans les langages objets en général), les mécanismes d'encapsulation sont directement intégrés au langage avec les modificateurs d'accès (`private`, `public`, `protected`).