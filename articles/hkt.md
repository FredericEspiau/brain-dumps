# Higher-kinded types

Il existe plusieurs couches pour le typage, on définit au moins 3 niveaux

# Niveau 0: les `value types

On entend aussi type simple ou type plat

On peut organiser les types en `kinds` (espèce ou genre en français)

Types qu'on peut attacher à des valeurs

```scala
val aNumber: Int = 42
val aString: String = "Scala"
```

On peut aussi typer des fonctions

```python
// int -> str
def stringify(arg: int) -> str:
    return str(arg)
```

C'est le genre/l'espèce (`kind`) le plus simple

On les appelle aussi parfois `*-kinded`

# Niveau 1: les `generics`

Permet d'associer un type dans un type

Exemple: les listes, pour indiquer de quel type sont les valeurs de la liste

```scala
class LinkedList[T]
class Optional[T]
```

Le type `T` est un `type argument` (argument de type)

On ne peut pas utiliser un type de niveau 1 sans préciser les types réels à utiliser en arguments

```scala
val aListOfNumbers: LinkedList[Int] = new LinkedList[Int]
val aListOfStrings: LinkedList[Int] = new LinkedList[String]
```

`LinkedList[Int]` est un `value type`, donc de niveau 0, car on peut l'attacher à une valeur

On dit que `LinkedList` est un `higher-level type` parce qu'on peut l'utiliser que si on lui passe un `level-0 type` comme argument

Les types de niveau 1 sont les espèces (`kind`) de types qui reçoivent des arguments de types du niveau inférieur (`level-0`)

L'espèce de `List` est `* -> *` car il n'a qu'un argument `List -> T = List[T]`
L'espèce de `Dictionnary` est `* -> * -> *` car il prend deux arguments `Dict -> K -> V = Dict[K, V]`

# Type constructors

On a attaché le type de niveau 1 `LinkedList` avec l'argument d e type de niveau 0 `Int`

Ca nous permis de créer un nouveau type de niveau 0

Ca ressemble à une fonction, on a créé un nouveau type pour transformer un type en un autre

C'est pour cela que les types génériques sont aussi appelée constructeurs de types, car ils peuvent créer des types de niveau 0

`LinkedList` est un constructeur de type, `Int` est un `type value` et on retourne un `value type` qui est `LinkedList[Int]`

# Utiliser les types de niveau 1

Je veux créer une fonction pour tranformer des nombres en strings

```python
def stringify_list_items(arg: List[int]) -> List[str]:
    return [str(item) for item in arg]
```

En revanche, je voudrais que cette fonction marche aussi avec les dictionnaires ou les stacks, comment faire ?

Une solution serait d'utiliser une interface commune entre `List`, `Dictionary` et `Stack`

```python
def stringify_iterable_items(arg: Iterable[int]) -> Iterable[str]:
    return type(arg)(str(item) for item in arg)
```

Par contre, la fonction nous retourne un `Iterable` et je perds par conséquent mon type initial, qui était `List`

On pourrait créer une méthode pour chaque type et préciser à chaque fois le type précis qu'on veut en retour mais ce n'est pas souhaitable pour plusieurs raisons

```python
@overload
def stringify_iterable_items(arg: List[int]) -> List[str]:

@overload
def stringify_iterable_items(arg: Set[int]) -> Set[str]:
    ...

def stringify_iterable_items(arg):
    return type(arg)(str(item) for item in arg)
```

# Le niveau 2 et au dessus: les `higher-kinded types`

Quand les arguments de types sont aussi des génériques

```scala
// the Cats library uses this A LOT
class Functor[F[_]]
```

On indique ici que l'argument de type `F` est un générique (niveau 1)

Comme l'argument est de niveau 1, alors le `Functor` est de niveau 2

```scala
val functorList = new Functor[List]
val functorOption = new Functor[Option]
```

`Functor` est aussi un constructeur de type

En Scala on peut embarquer récursivement les `[_]`

```scala
class Meta[F[_[_]]] // a level 3 type
```

Il est quand même rare d'aller au delà du niveau 2

# Aller plus loin

On peut avoir des constructeurs qui prennent plus d'un argument, qui peuvent être d'espèce différente

```
class HashMap[K, V] // type constructor avec 2 arguments de niveau 0
val anAddressBook = new HashMap[String, String]

class ComposedFunctor[F[_], G[_]] // type constructor avec 2 arguments de niveau 1
val aComposedFunctor = new ComposedFunctor[List, Option]

class Formatter[F[_], T] // type constructor avec 1 argument de niveau 1 et un autre de niveau 0
val aFormatter = new Formatter[List, String]
```

# Value constructor

Une valeur qui permet d'appliquer un ou des arguments pour construire une valeur

Ces arguments sont eux-mêmes des valeurs

On appelle souvent les `value constructor` des fonctions ou des méthodes

On dit que ces constructeurs sont
- polymorphiques parce qu'ils peuvent être utiliser pour construire des choses de différentes formes
- des abstractions comme ils permettent de faire abstraction de ce qui change entre plusieurs différentes instanciations polymorphiques

# First-order

Dans le contexte de l'abstraction et/ou du polymorphisme, first-order signifie qu'on abstrait sur un type une fois mais que ce type lui-même ne peut pas abstraire autre chose

Par exemple, les génériques sont first-order

# Type constructor

Type qui permet d'appliquer un ou des arguments pour construire un type

Ces arguments sont eux-mêmes des types

# First-order pour les constructeurs ci-dessus

Un type constructor est un type qu'on peut appliquer pour typer les arguments pour construire un type


Un value constructor est une valeur qu'on peut appliquer pour typer les arguments pour construire une valeur

# Sources:

- https://blog.rockthejvm.com/scala-types-kinds/
- https://sobolevn.me/2020/10/higher-kinded-types-in-python
