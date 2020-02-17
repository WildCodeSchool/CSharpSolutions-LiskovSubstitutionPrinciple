# C# - 1. Liskov Substitution Principle 

*Après cette quête, tu sauras reconnaître une violation du principe de substitution de Liskov et seras capable d'appliquer ce principe dans ton code*

## Objectifs

* Comprendre le principe de qualité logicielle
* Repérer une violation du principe de substitution de Liskov
* Concevoir des abstractions pertinentes

## Etapes

### Monsieur le géomètre, revoyez vos classiques !

Alors que tu es en cours de géométrie par l'informatique, ton prof t'explique qu'un carré ne devrait pas être un rectangle.

Outré par le fait d'avoir appris au collège qu'un carré est un rectangle, tu demandes des précisions à ton érudit professeur, prêt à t'apporter de la lumière sur le sujet.

Et il commence par t'expliquer que, si un carré était un rectangle, il y aurait violation du principe de substitution de Liskov...

![Geometry](https://upload.wikimedia.org/wikipedia/commons/thumb/1/13/Teorema_de_desargues.svg/1280px-Teorema_de_desargues.svg.png)

#### Ressources

### Enoncé du principe

Le principe de substitution de Liskov est une des pierres angulaires du modèle SOLID. Il s'agit d'un principe en somme assez simple, même si sa définition rigoureuse nous laisse sur notre faim :

Si **q(x)** est une propriété démontrable pour tout objet **x** de type **T**, alors **q(y)** est *vraie pour tout objet y de type* ***S*** tel que **S** est un sous-type de **T**.

Alors, t'as compris ? Voici le principe de substitution de Liskov dans toute sa splendeur. Une explication plus détaillée est sûrement requise afin de pouvoir aisément décortiquer et comprendre la définition énoncée plus haut.

#### Ressources

* [Wikipedia - LSP](https://en.wikipedia.org/wiki/Liskov_substitution_principle) Lire l'intro et plus si affinités

### Une abstraction réaliste

Nous, respectables développeurs, codons en suivant des principes, tout comme quelqu'un de bienveillant suivra un code moral de bon comportement avec autrui. Le comportement des personnes bienveillantes n'est spécifié dans aucun document et il en va du bon sens commun de se considérer comme étant bienveillant. Nous avons un avantage par rapport aux non-codeurs, nos principes sont écrits noir sur blanc, répétés et redéfinis par des membres de la communauté. Tout ce qu'il y a à faire pour devenir un développeur respectable (altruiste), c'est suivre ces principes ! Mais encore faut-il comprendre leur application...

Le principe de substitution de Liskov concerne les ***abstractions*** réalisées dans un programme via les *héritages*. Cette loi stipule que *tout enfant d'une classe* ***T*** (sous-type de T) *ne doit pas altérer les comportements de base de* ***T***.

Ce principe semble couler de source une fois lu, et pourtant, il est parfois bien difficile de créer un sous-type qui n'altère pas le parent. Je m'explique :

Lorsque l'on code, on imagine une conception la plus proche de la réalité pour que le code puisse être partagé avec d'autres développeurs qui peuvent le faire évoluer. Très souvent, nous allons créer des *classes abstraites* qui décrivent le comportement que doivent avoir tous ses enfants.

Le problème est que, parfois, les abstractions sont ***mal choisies*** et ça ne se voit pas au premier abord.

![Abstraction canard](https://i.stack.imgur.com/ilxzO.jpg)

#### Ressources

* [Article sur refactoring LSP](https://code-maze.com/liskov-substitution-principle/)

### Un carré n'est pas un rectangle !

Prenons un exemple frappant décrit par Robert Martins :

En mathématiques, on stipule qu'un `rectangle` est un `carré`. On pourrait donc penser de la même manière, dès lors que l'on conçoit un programme manipulant ces deux polygones, mais cela violerait le principe de substitutivité de Liskov.

> Et pourquoi ça violerait un principe si la conception est proche de la réalité ?

Eh bien tout simplement parce qu'un carré est un rectangle ayant ses longueur et largeur égales.

> Oui, et donc ? C'est comme ça en vrai, non ?

Oui, dans la réalité nous fonctionnons comme ça, mais dans le code c'est légèrement différent. Dès lors que tu crées une classe abstraite, grâce au mécanisme de polymorphisme, tu peux utiliser *tous les enfants* de la classe abstraite en question de la même manière, malgré leurs différences.

Imagine maintenant une classe `Square` héritant d'une classe `Rectangle`. La classe `Rectangle` implémente deux méthodes `SetWidth` et `SetHeight` qui sont des accesseurs pour modifier respectivement les largeur et hauteur du rectangle. L'appel à `SetWidth` modifie uniquement la largeur, et l'appel à `SetHeight` modifie uniquement la hauteur. Qu'en est-il du comportement de ces méthodes dans le cas d'un carré - figure où doivent être égales hauteur et largeur - pour être qualifiées en tant que telles ?

> Eh bien c'est simple, dès lors qu'on modifie la largeur, ça affecte automatiquement la hauteur à la même largeur et le tour est joué !

Eh bien non, ceci est une violation du principe de substitution de Liskov, car maintenant le comportement de base du parent de `Square` est altéré. Puisque `Square` **est un** `Rectangle`, les accesseurs fournis par la classe `Rectangle` doivent avoir le même comportement au travers de ses enfants, or ce n'est plus le cas pour un carré dont on modifie les largeur et hauteur. Découlent alors des résultats inattendus dès lors qu'on manipule un `Square` comme un `Rectangle`.

#### Ressources

* [Article - Carré pas rectangle](https://www.infragistics.com/community/blogs/b/dhananjay_kumar/posts/simplifying-the-liskov-substitution-principle-of-solid-in-c)

## Challenge

Pour prouver à ton professeur que tu as bien compris qu'un carré ne doit pas être abstrait comme étant un rectangle, il te demande de le prouver en corrigeant un code qui implémente les deux figures en question (rectangle et carré) et qui contient une violation du principe de substitution de Liskov.

Voici le code violant le LSP que tu dois corriger :

```C#
using System;

namespace Geometry
{
    class Rectangle
    {
        private double width;
        private double height;

        public void SetWidth(double width)
        {
            this.width = width;
        }

        public void SetHeight(double height)
        {
            this.height = height;
        }

        public double GetWidth()
        {
            return width;
        }

        public double GetHeight()
        {
            return height;
        }
    }

    class Square : Rectangle
    {
        public void SetWidth(double width)
        {
            // Because a square must have its sides equal, I also set
            // its height to the given width
            this.width = width;
            this.height = width;
        }

        public void SetHeight(double height)
        {
           // Because a square must have its sides equal, I also set
           // its width to the given height
           this.height = height;
           this.width = height; 
        }
    }
}
```

### Critères de validation

* `Square` n'hérite plus de `Rectangle`
* `Square` est cependant un polygone au même titre que `Rectangle`
* `Square` et `Rectangle` héritent tous deux de la même classe-mère représentant un polygone