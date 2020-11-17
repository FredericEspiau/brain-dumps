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

Fin des années 90s, on utilisait des containers auxquelles on disait: j'ai ces dépendanes, tu peux me donner des implémentations concrètes stp ?

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

Cette fonction `map` respecte certaines propriétés

```scala
xs.map(identity) == xs
xs.map(f).map(g) == xs.map(x => g(f(x))
```
