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

C'est juste qu'il s'appelle pas `map`

```scala
def add(x: Int): Int => Int = _ + x
val f = add(2) andThen (_ * 3)
f(3) // 15
```

Mais on peut pas faire

```scala
val f = add(2) map (_ * 3)
```

Du coup on peut pas faire de `for comprehension`

## The Reader Monad

C'est un wrapper sur une fonction unaire avec `andThen` comme `map` et qui définit un `flatMap`

```scala
case class Reader[A, B](run: A => B) {
  def apply(x: A): B = run(x)
  
  def map[C](f: B => C): Reader[A, C] = Reader(run andThen f)
  
  def flatMap[C](f: B => Reader[A, C]): Reader[A, C] = Reader(x => map(f)(x)(x))
}
```

`scala` a déjà un bon reader de base

```scala
import scalaz.Reader

val f = Reader[Int, Int](_ + 2)

val g = f map (_ * 3)

g(3) // 15
```

Ca nous permet de faire des `map` et ainsi d'utiliser les `for comprehensions`

```scala
val g = for (x <- f) yield x * 3

g(3) // 15
```

## Dep injection

Un autre reader:

```scala
def getUser(userId: Int) =
  Reader[UserRepo, User](_.get(userId))
```

se wrap autour d'une fonction qui à partir d'un UserRepo obtient un User

le UserRepo est une dépendance, avec Reader on peut injecter cette dépendance

Comme Reader est une monade, elle est composable

```scala
def getEmail(userId: Int) =
  for (user <- getUser(userId))
    yield user.email
```

et avec le `flatMap` on peut même imbriquer et obtenir un User du repo et obtenir un autre user du repo

```scala
def getSupervisor(userId: Int) =
  for {
    user <- getUser(userId)
    supervisor <- getUser(user.supervisorId)
  } yield supervisor
```

On peut avoir une dépendance avec un certains nombre d'opérations et définir des Readers pour chaque opérations

```scala
trait UserRepo {
  def get(userId: Int): User
  def find(email: String): User
  def update(user: User): User
}

object UserRepo {
  def getUser(userId: Int) =
    Reader[UserRepo, User](_.get(userId))
    
  def findUser(email: String) =
    Reader[UserRepo, User](_.find(email))
    
  def updateUser(user: User) =
    Reader[UserRepo, User](_.update(user))
}

// on aurait pu écrire

object UserRepo {
  var userRepo = 
    Reader[UserRepo, User](identity)
    
  def getUser(userId: Int) =
    userRepo map (_.get(userId))
    
  def findUser(email: String) =
   userRepo map (_.find(email))
    
  def updateUser(user: User) =
    userRepo map (_.update(user))
}
```

# Other dependencies ?

En général on a pas qu'un seul repository

Il est courant de les aggréger ensemble

```scala
trait Repositories {
  def userRepo: UserRepo
  def addressRepo: AddressRepo
}

object Repositories {
  val repositories =
    Reader[Repositories, Repositories](identity)
    
  val userRepo =
    repositories map (_.userRepo)
   
  val addressRepo =
    repositories map (_.addressRepo)
}
```

Du coup, la seule chose à changer dans notre UserRepository est de ne rien déclarer et d'importer le repository

```scala
object UserRepo {
  import Repositories.userRepo
  
  def getUser(userId: Int) =
    userRepo map (_.get(userId))
    
  def findUser(email: String) =
   userRepo map (_.find(email))
    
  def updateUser(user: User) =
    userRepo map (_.update(user))
}
```

On peut aller encore plus loin et déclarer toutes nos dépendances dans un trait environnemental

```scala
trait Env {
  def config: Configuration
  def emailService: EmailService
  def repositories: Repositories
}

object Env {
  val env = Reader[Env, Env](identity)
  
  val config =
    env map (_.config)
    
  val emailService =
    env map (_.emailService)
    
  val repositories =
    env map (_.repositories)
}
```

On a plus qu'un seul `Reader` pour tout l'environement qui va nous permettre de créer des `Reader` pour chaque dépendance

Nos services ressemblent désormais à ça

```scala
```
On a plus qu'un seul `Reader` pour tout l'environement qui va nous permettre de créer des `Reader` pour chaque dépendance
