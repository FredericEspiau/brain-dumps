# The Reader Monad for Dependency Injection

Source: https://www.youtube.com/watch?v=xPlsVVaMoB0

# Why dependency injection ?

Service Layer -> Persistence Layer

Une layer qui dépend d'une autre layer

Du coup couplage entre la layer de haut niveau (service) et la layer de bas niveau (persistence)

Du coup changement dans la layer de bas niveau implique changement layer haut niveau

La couche de service ne peut pas exister sans la couche de persistence

Bob Martin a inventé une solution, le DIP

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

On a plus qu'un seul `Reader` pour tout l'environnement qui va nous permettre de créer des `Reader` pour chaque dépendance

Nos services ressemblent désormais à ça

```scala
object UserService {
  def getEmail(userId: Int) =
    for {
      user <- UserRepo.getUser(userId)
    } yield user.email
    
  def findAddress(email: String) =
    for {
      user <- UserRepo.findUser(email)
      address <- AddressRepo.getAddress(user.id)
    } yield address
}
```

On a plus qu'un seul `Reader` pour tout l'environnement qui va nous permettre de créer des `Reader` pour chaque dépendance

On peut mettre toutes les dépendances et leurs composants ainsi, afin de pouvoir définir la nature de l'environnement en mixant des traits

```scala
trait ConfigurationComponent {
  def config: Configuration
}

trait EmailServiceComponent {
  def emailService: EmailService
}

trait Env extends
 ConfigurationComponent with
 EmailServiceComponent with
 RepositoriesCompoent
 
trait RepositoriesComponent extends
  UserRepoComponent with
  AddressRepoComponent
```

permet de définir des implémentations des composants dans des traits différents en fonction de l'environnement

```scala
object productionEnv extends Env
  with PlayConfigComponent
  with PlayEmailServiceComponent
  with MongoRepositoriesComponent
  
object testEnv extends Env
  with MockConfigComponent
  with MockEmailServiceComponent
  with MockRepositoriesComponent
```

## Other monades

Avec le framework Scala Play, tout est asyncho du coup un repo ressemble à ça

```scala
trait UserRepo {
  def get(userId: Int): Future[User]
  def find(email: String): Future[User]
  def update(user: User): Future[User]
}
```

`Future` est une monade, du coup avec `Reader` on peut avoir un bordel entrelacé avec `flatMap` pour l'un et `map` pour l'autre

Par exemple,

```scala
def getEmail(userId: Int) =
  for (user <- getUser(userId))
    yield userFuture map (_.email)
```

comme `getUser` retourne un `Future`, il faut faire un map sur le Reader pour avoir le Future et map sur le Futur pour savoir l'email

Et quand on a un multi-level comprehension on doit faire des flatMap c'est encore plus compliqué

```scala
def findAddress(email: String) =
  Env.env map { env =>
    for {
      user <- UserRepo.findUser(email).run(env)
      address <- AddressRepo.getAddress(user.id).run(env)
    } yield address
  }
```

Trop de boilerplate, la solution s'appelle

## Monade transformer

Le problème c'est que notre méthode a une signature `Reader[Env, Future[User]]`, par exemple

```scala
def findUser(email: String): Reader[Env, Future[User]]
```

ce qui donne une `class` telle que

```scala
class Reader[Env, Future[A]] {
  def map[B](f: Future[A] => B):
    Reader[Env, Future[B]]
    
 def flatMap[B](f: Future[A] => Reader[Future[B]]):
   Reader[Env, Future[B]]
}
```

Le problème c'est le paramètre de type `Future[A]`

on voudrait plutôt avoir ça

```scala
class Something[A] {
  def map[B](f: A => B):
   Something[B]
   
 def flatMap[B](f: A => Something[B]):
   Something[B]
}
```

On peut faire ça en changeant `Reader[Env, Future[User]]` en `ReaderT[Future, Env, User]` dans notre méthode

```scala
class ReaderT[Future, Env, A] {
  def map[B](f: A => B):
    ReaderT[Future, Env, B]
    
  def flatMap[B](f: A => ReaderT[Future, Env, B]):
    ReaderT[Future, Env, B]
}
```

ce qui nous permet d'avoir une implem comme ça

```scala
def findAddress(email: String) =
  for {
    user <- UserRepo.findUser(email)
    address <- AddressRepo.getAddress(user.id)
  } yield address
```

Avec Scalaz ça donne

```scala
import scalaz.{Kleisli, ReaderT}

type Query[A] = ReaderT[Future, Env, A]

object Query {
  def apply[A](run: Env => Future[A]): Query[A] =
    Kleisli[Future, Env, A](run)
    
  def lift[A](reader: Reader[Env, Future[A]] =
    Query(reader.run)
}
```

On peut ainsi définir notre répo comme ça

```scala
object UserService {
  import scalaz.contrib.std.scalaFuture._
  
  def getEmail(userId: Int)(implicit ec: ExecutionContext) =
    for {
      user <- UserRepo.getUser(userId)
    } yield user.email
}
```

Ce serait encore mieux d'avoir l'ExecuteContext dans l'environnement et utiliser la Reader monade

## Dependency injection ?

Comment on met les dépendances dans le Reader dans une vrai app ?

Exemple

```scala
import scalaz.Reader

abstract class EnvController(env: Env) extends Controller {
  def run[A](r: Reader[Env, A]): A = r.run(env)
  def run[A](query: Query[A]): Future[A] = query.run(env)
}
```

On a un controller abtrait qui prend l'environnement dans le constructeur et a des fonctions run qui executent le reader sans environnement

Du coup on applique juste l'environnement à la fonction de la monade qui est englobée (wrapped)

```scala
class Users(env: Env) extends EnvController(env) {
  import scalaz.contrib.std.scalaFuture._
  import Execution.Implicit.defaultContext
  
  def show(id: String) = Action.async { request =>
    for (user <- run(UserService.getUser(id)))
      yield Ok(views.html.user(user))
  }
}
```

Le vrai controller va étendre ce controller abstrait et va exécuter la fonction `run` 

On n'élimine pas l'appel à `run` parce qu'on n'essaie pas d'avoir uniquement de la logique applicative dans le controller

Le framework Play a des features sympas pour utiliser l'injection de dépendance, par exemple

```scala
object Global extends GlobalSettings {
  private object env extends Env 
    with PlayConfigComponent
    with PlayEmailServiceComponent
    with MongoRepositoriesComponent
    
  override def getControllerInstance[A](c: Class[A]) =
    c.getConstrustor(classOf[Env]).newInstance(env)
}
```

on a juste à override `getControllerInstance` en utilisant la réflexion 

Dans les routes, on peut juste ajouter un `@` pour indiquer qu'il faut utiliser l'injection de dépendance

```scala
GET /users/:id @controllers.Users.show(id: String)
```

## Testing

Une motivation de l'injection de dépendance est de faciliter les tests

```scala
trait MockerRepositoriesComponent extends RepositoriesComponent {
  object repos extends Repos
    with MockUserRepoComponent
    with MockAddressRepoComponent
}

trait MockUserRepoComponent extends UserRepoComponent {
  val userRepo = mock[UserRepo]
}
```

On peut ensuite créer un environnement de mock

```scala
class MockEnv extends Env
  with MockConfigComponent
  with MockEmailServiceComponent
  with MockRepositoriesComponent
  
trait MockConfigComponent extends ConfigComponent {
  val config = mock[Config]
}
```

On peut créer un environnement de test ainsi

```scala
trait TraitEnv {
  import Helpers._
  
  def env: Env
  
  def config = env.config
  def emailService = env.emailService
  def repositories = env.repositories
  def userRepo = repositories.userRepo
  def addressRepo = repositories.addressRepo
  
  def await[A](query: Query[A]): A =
    Helpers.await(query.run(env))
}
```

## To read

https://www.fpcomplete.com/blog/2017/06/readert-design-pattern/
http://anttih.com/articles/2018/07/05/purely-functional-di
https://medium.com/rahasak/dependency-injection-with-reader-monad-in-scala-fe05b29e04dd
https://softwaremill.com/reader-monad-constructor-dependency-injection-friend-or-foe/
https://www.google.com/search?ei=JKnEX6ioDY-9lwSK5KyQDw&q=reader-monad-for-dependency-injection&oq=reader-monad-for-dependency-injection&gs_lcp=CgZwc3ktYWIQAzIECAAQRzIECAAQRzIECAAQRzIECAAQRzIECAAQRzIECAAQRzIECAAQRzIECAAQR1DPC1jPC2C4DGgAcAJ4AIABAIgBAJIBAJgBAKABAaoBB2d3cy13aXrIAQjAAQE&sclient=psy-ab&ved=0ahUKEwioypPv6KntAhWP3oUKHQoyC_IQ4dUDCA0&uact=5
