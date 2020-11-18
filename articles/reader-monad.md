# The Reader Monad for Dependency Injection

Source: https://www.youtube.com/watch?v=xPlsVVaMoB0

# Why dependency injection ?

Service Layer -> Persistence Layer

Une layer qui dépend d'une autre layer

Du coup couplage entre la layer de haut niveau (service) et la layer de bas niveau (persistence)

Du coup changement dans la layer de bas niveau implique changement layer haut niveau

La couche de service ne peut pas exister sans la couche de persistence

Bob Martin a inventé une oslution, le DIP

- High level modules should not depend upon low level modules

Both should depend upon abstractions

- abstraction should not depend upon details

Details should depend upon abstractions

En d'autress termes, il faut mettre une abstraction entre les couches de bas niveau et celles de haut nivau

Service layer -> Persistence Interface <- Persistence Layer

Du coup on a un nouveau problème, comment on lie les layers entre elles ? On aura besoin à un moment ou à un autre d'une implémentation concrète

Fin des années 90s, on utilisait des containers auxquelles on disait: j'ai ces dépendances, tu peux me donner des implémentations concrètes stp ?

Ce qui donne l'inversion de contrôle et l'injection de dépendance, ça veut dire que c'est la responsabilité du container de fournir les dépendances

# Monades in Scala

La liste, on peut la créer et faire des maps avec

```scala
val xs = List(1, 2, 5)
xs map (x => x * x) // => List(1, 4, 25)

// on a aussi des comprehensions

for (x <- xs) yield x * x
```

La signature de map est celle là

```scala
class List[A] {
  def map[B](f: A => B): List[B]
}
```

On prend une `List[A]`, on applique la fonction qui `A -> B` pour obtenir une `List[B]`

Cette fonction `map` respecte certaines propriétés

```scala
xs.map(identity) == xs // identity
xs.map(f).map(g) == xs.map(x => g(f(x)) // associativity
```

Cette signature et ces propriétés donnent un `Functor`

On peut aussi `flatmap` une `List`

```scala
val xs = List(2, 5)
xs flatMap (x => List(x, x + 1)) // List(2, 3, 5, 6)

---

val xs = List(1, 2)
val ys = List(3, 4)

// multilevel comprehension
for {
  x <- xs
  y <- ys
} yield x * y
```

`flatMap` a pour signature

```scala
class List[A] {
  def flatMap[B](f: A => List[B]): List[B]
}
```

on a une relation avec map telle que

```scala
xs.flatMap(x => List(f(x))) == xs.map(f)
```

On a aussi des propriétés

```scala
List(x).flatMap(f) == f(x) // I
xs.flatMap(x => List(x)) == xs // I
xs.flatMap(f).flatMap(g) == xs.flatMap(f(_).flatMap(g)) // A
```

- La signature de `map` et `flatMap`
- Les règles qu'elles respectent

font de `List` une monade

# Function

Fonction unaire: un seul argument

```scala
def add(x: Int): Int => Int = _ + x
val add2 = add(2)
add2(3) // 5

def multiplyBy(x: Int): Int => Int = _ * x
val f = add(2) andThen multiplyBy(3)
f(3) // 15
```

`andThen` a aussi I et A, comme `Functor`, du coup les fonctions Scala composées avec `andThen` sont des functors

