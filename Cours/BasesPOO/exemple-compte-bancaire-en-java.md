# Compte bancaire en Java ?

Pour vous donner une idée, voilà comment on pourrait écrire en Java une classe Compte et comment on pourrait l'instancier et l'utiliser. On verra de façon plus précise dans la suite de ce cours toutes ces notions.

```java
public class Compte {
    private int solde; // On déclare l'entier solde comme attribue privé, non visible à l'extérieur
    
    // Constructeur d'objet
    public Compte(int soldeInitial) {
        this.solde = soldeInitial;
    }
    
    // Méthodes publiques
    public void retirer(int somme) {
        if (somme > 0 && somme <= this.solde) { // La somme foit être positive et inférieur au solde
            this.solde -= somme;
        }
    }
    
    public void deposer(int somme) {
        if (somme > 0) { // On ne peut déposer qu'une somme positive
            this.solde += somme;
        }
    }
    
    public int obtenirSolde() {
        return this.solde;
    }    
}
```


Maintenant que la classe Compte est définie, on va voir comment elle peut être utilisé depuis *"l'extérieur"*, c'est à dire depuis une autre partie d'un programme Java. Encore une fois, on rentrera plus tard dans les détails, ces extraits de code ont pour seul but de vous familiariser avec la syntaxe et servent à illustrer notre propos sur l'encapsulation, les objets et (un peu) les classes.

```java
public class TestCompte {
    // Méthode main pour tester
    public static void main(String[] args) {
        Compte monCompte = new Compte(1500); // Création d'un nouvel objet Compte, avec un solde initial de 1500

        monCompte.solde = 30; // Cette ligne ne compile pas, accès direct impossible à solde, le compilateur nous protège d'une mauvaise manipulation car l'attribut solde est privé.

        monCompte.deposer(500);   // On dépose 500, on a 2000
        monCompte.retirer(300);   // On retire 300, on a 1700
        monCompte.retirer(2000);  // Tentative de retrait supérieur au solde
    }
}
```