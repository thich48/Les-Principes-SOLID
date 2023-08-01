# Les Principes SOLID

En programmation informatique, **SOLID** est un acronyme représentant cinq
principes de base pour la programmation orientée objet. Ces cinq principes sont
censés apporter une ligne directrice permettant le développement de logiciels
plus fiables, plus robustes, plus maintenables, plus extensibles et plus
testables.

---

## **S**ingle Responsability Principle

* **S**ingle Responsability Principle ou « principe de responsabilité unique »
  qui stipule qu&#39;une classe doit avoir une et une seule responsabilité. Si l&#39;on
  parvient à identifier dans une classe au moins deux raisons valables de la
  modifier, c&#39;est que cette classe dispose d&#39;au moins deux responsabilités.

---

Prenons l&#39;exemple d&#39;un module qui compile et imprime un rapport. Imaginons que ce module peut changer pour deux raisons. D&#39;abord, le contenu du rapport peut changer. Ensuite, le format du rapport peut changer. Ces deux choses changent pour des causes différentes; l&#39;une substantielle, et l&#39;autre cosmétique. Le principe de responsabilité unique dit que ces deux aspects du problème ont deux responsabilités distinctes, et devraient donc être dans des classes ou des modules séparés. Ce serait une mauvaise conception de coupler ces deux choses dans une même classe.

La raison pour laquelle il est important de garder une classe axée sur une seule préoccupation est que cela rend la classe plus robuste. En continuant avec l&#39;exemple précédent, s&#39;il y a un changement dans le processus de compilation du rapport, il y a un plus grand danger que le code d&#39;impression se casse si elle fait partie de la même classe.

---

## **O**pen Close Principle

* **O**pen Close Principle ou « principe Ouvert / Fermé » qui stipule qu&#39;une
  classe doit être ouverte à l&#39;extension et fermée aux modifications. En
  d&#39;autres termes, il doit être possible de facilement enrichir les capacités
  d&#39;un objet sans que cela implique de modifier le code source de sa classe. Des
  patrons de conception comme la *Stratégie* ou le *Décorateur* répondent à ce
  principe en favorisant la composition d&#39;objets plutôt que l&#39;héritage.

---

Une classe, une méthode, un module doit pouvoir être étendu, supporter différentes implémentations (Open for extension) sans pour cela devoir être modifié (closed for modification).

Les instanciations conditionnelles dans un constructeur sont de bons exemples de non respect de ce principe. Une nouvelle implémentation aura pour impact l’ajout d’une condition dans la méthode.

Voyons sur un exemple, la violation de ce principe :

```Java
Public Car(EngineTypeEnum engineType) {
    if (engineType == EngineTypeEnum.FUEL) {
        Engine = new FuelEngine() ;
    } else if (…) {
        …
    }
}
````

Dans cet exemple nous voyons que l’ajout d’un type d’Engine va entraîner une modification du constructeur : nous sommes en violation avec la deuxième partie du principe – Closed for modification.

Mais comment respecter ces deux notions en même temps ?

Plusieurs solutions s’offrent à nous. La première consiste à utiliser l’injection de dépendance dans sa forme la plus simple :

```Java
public Car (Engine engine) {
   this.engine = engine ;
}
````

Il sera préférable d’utiliser une fabrique d’objets ou de l’injection de dépendance pour réduire le couplage.

---

## **L**iskov Substitution Principle

* **L**iskov Substitution Principle ou « principe de substitution de Liskov »
  qui définit qu&#39;une instance de type T doit pouvoir être remplacée par une
  instance de type G, tel que G sous-type de T, sans que cela modifie la
  cohérence du programme. En d&#39;autres termes, il s&#39;agit de conserver les mêmes
  prototypes ainsi que les mêmes conditions d&#39;entrée / sortie lorsqu&#39;une classe
  dérive une classe parente et redéfinit ses méthodes. Cela vaut aussi pour les
  types d&#39;exceptions. Si une méthode de la classe parente lève une exception de
  type `E` alors cette même méthode redéfinie dans une sous-classe `C'` qui
  hérite de `C` doit aussi lever une exception de type `E` dans les mêmes
  conditions.

---

L&#39;exemple classique d&#39;une violation du LSP est la suivante :

Soit une classe Rectangle représentant les propriétés d&#39;un rectangle : hauteur, largeur. On lui associe donc des accesseurs pour accéder et modifier la hauteur et la largeur librement. En postcondition, on définit la règle : la hauteur et la largeur sont librement modifiables.
Soit une classe Carré que l&#39;on fait dériver de la classe Rectangle. En effet, en mathématiques, un carré est un rectangle. Donc, on définit naturellement la classe Carré comme sous-type de la classe Rectangle. On définit comme postcondition la règle : les « quatre côtés du carré doivent être égaux ».
On s&#39;attend à pouvoir utiliser une instance de type Carré n&#39;importe où un type Rectangle est attendu.

**Problème :** Un carré ayant par définition quatre côtés égaux, il convient de restreindre la modification de la hauteur et de la largeur pour qu&#39;elles soient toujours égales. Néanmoins, si un carré est utilisé là où, comportementalement, on s&#39;attend à interagir avec un rectangle, des comportements incohérents peuvent subvenir : les côtés d&#39;un carré ne peuvent être changés indépendamment, contrairement à ceux d&#39;un rectangle. Une mauvaise solution consisterait à modifier les setter du carré pour préserver l&#39;invariance de ce dernier. Mais ceci violerait la postcondition des setter du rectangle qui spécifie que l&#39;on puisse modifier hauteur et largeur indépendamment.

Une solution pour éviter ces incohérences est de retirer la nature Mutable des classes Carré et Rectangle. Autrement dit, elles ne sont accessibles qu&#39;en lecture. Il n&#39;y a aucune violation du LSP, néanmoins on devra implémenter des méthodes &#34;hauteur&#34; et &#34;largeur&#34; à un carré, ce qui, sémantiquement, est un non sens.

**Solution :** La solution consiste à ne pas considérer un type Carré comme substitut d&#39;un type Rectangle, et les définir comme deux types complètement indépendants. Ceci ne contredit pas le fait qu&#39;un carré soit un rectangle. La classe Carré est un représentant du concept « carré ». La classe Rectangle est un représentant du concept « rectangle ». Or, les représentants ne partagent pas les mêmes propriétés que ce qu&#39;ils représentent[3].

---

## **I**nterface Segregation Principle

* **I**nterface Segregation Principle ou « principe de ségrégation d&#39;interface »
  qui recommande de découper de grosses interfaces en plus petites interfaces
  spécialisées. Ainsi les classes clientes peuvent implémenter une ou plusieurs
  petites interfaces spécialisées plutôt qu&#39;une grosse interface afin d&#39;obtenir
  seulement les méthodes dont elles ont besoin.

---

Le but de ce principe est d’utiliser les interfaces pour définir des contrats, des ensembles de fonctionnalités répondant à un besoin fonctionnel, plutôt que de se contenter d’apporter de l’abstraction à nos classes. Il en découle une réduction du couplage, les clients dépendant uniquement des services qu’ils utilisent.

L’utilisation systématique d’interface de type IMaClasse reprenant les méthodes publiques de la classe MaClasse n’est par conséquent pas une bonne pratique car cela lie nos contrats à leur implémentation, rendant délicat la réutilisation et les refactorings à venir.

Une mise en garde cependant : un des travers de ce principe peut être de multiplier les interfaces. En poussant cette idée à l’extrême, nous pouvons imaginer une interface avec une méthode par client. Bien entendu, l’expérience, le pragmatisme et le bon sens sont nos meilleurs alliés dans ce domaine.

---

## **D**ependency Inversion Principle

* **D**ependency Inversion Principle ou « principe d&#39;inversion des dépendances »
  qui stipule que les objets doivent dépendre d&#39;abstractions plutôt que
  d&#39;implémentations. Cela signifie qu&#39;il est préférable de typer des arguments
  avec des types abstraits (classes concrètes ou interfaces) plutôt que
  des types concrets (classes concrètes) afin de diminuer les couplages
  et favoriser d&#39;autres implémentations.

---

Attardons-nous sur la notion importante de ce principe : Inversion. Le principe de DIP stipule que les modules de haut niveau ne doivent pas dépendre de modules de plus bas niveau. Mais pour quelle raison ? Pour répondre à cette question, prenons la définition à l’envers : les modules de haut niveau dépendent de modules de bas niveau. En règle générale les modules de haut niveau contiennent le cœur – business – des applications. Lorsque ces modules dépendent de modules de plus bas niveau, les modifications effectuées dans les modules « bas niveau » peuvent avoir des répercussions sur les modules « haut niveau » et les « forcer » à appliquer des changements.


Pour savoir si le code d’une application ne respecte pas les principes SOLID et s’il est nécessaire de les appliquer, il est possible d’utiliser les métriques structurelles d’instabilité (relative au couplage) et d’abstraction pour calculer le rapport entre le degré d’utilisation d’un module et son niveau d’abstraction. Un module sera d’autant plus stable qu’il aura de dépendances entrantes par rapport à ses dépendances sortantes.

Le « Stable Abstraction Principle » part du principe que les packages stables doivent être abstraits.

Un module majoritairement utilisé par d’autres (défini comme stable) devra avoir un niveau d’abstraction plus important. Inversement, un module qui n’est utilisé par aucun autre, n’aura aucun intérêt à être abstrait.
