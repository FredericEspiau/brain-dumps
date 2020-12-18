# Application partielle et curryfication

## Préambule

Cet article utilise `TypeScript` mais les concepts abordés sont applicables à de nombreux langages

Voici une courte explication de la syntaxe de `TypeScript`

```ts
const areTheSame = (a: number, b: string): boolean => a.toString() === b
```

Cette instruction

- crée une fonction appelée `areTheSame`
- la fonction `areTheSame` déclare deux paramètres `a` et  `b`
- le paramètre `a` est de type `number`
- le paramètre `b` est de type `string`
- la fonction `areTheSame` retourne un `boolean`
- la partie derrière le `=>` est le corps de la fonction

Il est possible de créer des fonctions qui renvoient des fonctions de cette manière

```ts
const areTheSameInTwoFunctions = (a: number) => (b: string): boolean => a.toString() === b
```

Cette instruction

- crée une fonction appelée `areTheSameInTwoFunctions`
- la fonction `areTheSameInTwoFunctions` déclare un paramètres `a`
- le paramètre `a` est de type `number`
- le corps de `areTheSameInTwoFunctions` va renvoyer une nouvelle fonction anonyme (qui n'a pas de nom)
- cette fonction anonyme déclare un paramètre `b`
- le paramètre `b` est de type `string`
- la fonction anonyme retourne un `boolean`

Les deux fonctions peuvent être exécutées ainsi

```ts
const theyAreTheSame = areTheSame(1, "1");
const theyAreNotTheSame = areTheSameInTwoFunctions(1)("2")
```

## Différence entre `paramètre` et `argument`

Un `paramètre` se trouve dans la déclaration d'une fonction pour indiquer de quoi elle a besoin pour être exécutée

Un `argument` est une valeur concrète passée à un `paramètre`

```ts
const increment = (a: number): number => a + 1
increment(5);
```

`a` est un paramètre
`5` est un argument

## Définition du terme `arité`

Nombre d'arguments dont une fonction a besoin pour être executée

```ts
const addThree = (a: number, b: number, c: number): number => a + b + c
```

La fonction `addThree` a besoin qu'on attribue des valeurs à ses `3` paramètres `a`, `b` et `c` pour être executée

```ts
const six = addThree(1, 2, 3);
```

Son arité est donc `3`

## Définition du terme `unaire`

Fonction dont l'arité est `1`

```ts
const increment = (n: number): number => n + 1;
```

La fonction `increment` a besoin qu'on attribue une valeur à son argument `n` pour être executée

```ts
const two = increment(1);
```

Son arité est donc `1`

## Définition du terme `application partielle`

Produire une nouvelle fonction `B` à partir d'une fonction `A`. L'arité de la fonction `B` doit être inférieure à l'arité de la fonction `A`

```ts
const addThreeNumbers = (a: number, b: number, c: number): number => a + b + c
const addTenToTwoNumbers = (b: number, c: number): number => add(10, b, c);
```

La fonction `addThreeNumbers` a une arité de `3`

La fonction `addTenToTwoNumbers`, créée à partir de `addThreeNumbers`, a une arité de `2`

Il s'agit donc d'application partielle

Le fait de donner une valeur à un argument afin d'effectuer une application partielle s'appelle un `fix` ou un `bind`

En `JavaScript` et `TypeScript`, il est possible d'appliquer partiellement une fonction avec `bind`

```ts
const addTenToTwoNumbers = (b: number, c: number): number => addThreeNumbers.bind(null, 10);
```

Cette technique a le désavantage de vous obliger à fournir un contexte pour votre nouvelle fonction (`null` dans l'exemple précédent) et ne fonctionne qu'avec le premier argument de votre fonction (il n'aurait pas été possible de créer un application partielle sur `b` ou `c`)

## A quoi ça sert ?

Une fonction appliquée partiellement a certains de ses arguments fixés et est en attente d'un ou de plusieurs arguments pour pouvoir s'exécuter

Par conséquent, une fonction appliquée partiellement est une fonction qui contient de l'information

Cette fonction peut ensuite être utilisée pour créer de nouvelles fonctions, avec les informations déjà fixée en argument

Par conséquent, une fonction appliquée partiellement est capable de créer de nouvelles fonctions qui partageront de l'information

```ts
const buildUrl = (procotol: string, domain: string, path: string) => protocol + "://" + domain + / + path;
const buildSecureUrl = (domain: string, path: string): string => buildUrl("https", domain, path);
```

Je peux réutiliser `buildSecureUrl` pour construire des valeurs ou des fonctions qui partageront le fait que le protocole à utiliser est `https`

```ts
const buildWebsiteUrl = (path: string): string => buildSecureUrl("website.com", path);
const buildAppUrl = (path: string): string => buildSecureUrl("app.website.com", path);
const urlToInternshipOffer = buildSecureUrl("careers.website.com", "offer?id=10");
```

On utilise ainsi l'application partielle pour éviter de se répéter ou pour éviter d'effectuer des calculs coûteux plusieurs fois

```ts
const myFunction = (arg1, arg2) => {
  const complexResult = functionThatTakesTime(arg1);
  
  return complexResult + arg2;
}

const otherFunction = myFunction(1, 3);
const thirdFunction = myFunction(1, 2);
```

Le calcul qui prend du temps est effectué deux fois alors qu'avec de l'application partielle, on pourrait ne l'effectuer qu'une seule fois

```ts
const myFunction = (arg1) => {
  const complexResult = functionThatTakesTime(arg1);
  
  return (arg2) => complexResult + arg2;
}

const alreadyDone = myFunction(1);
const otherFunction = alreadyDone(3);
const thirdFunction = alreadyDone(2);
```

Le calcul qui prend du temps n'est effectué qu'une seule fois

On pourrait passer par une variable intermédiaire qui contiendrait le résultat mais ce serait se priver de la composabilité des fonctions

## Définition du terme `curryfication`

Prenons l'exemple de la fonction suivante

```ts
const add = (a: number, b: number): number => a + b
```

`add` n'est pas unaire parce que son arité est de `2`

Curryfier `add` signifierait

- obtenir à partir de `add` une nouvelle fonction
- qui aurait pour but d'effectuer la même action que `add`
- qui serait unaire
- qui retournerait une fonction elle-même currifiée pour gérer les arguments restants

```ts
const add = (a: number, b: number): number => a + b
const addCurried = (a: number) => (b: number): number => a + b
```

La fonction `addCurried` est une fonction unaire, elle a pour seul argument `a`

Elle retourne une fonction unaire, elle a pour seul argument `b` et elle effectue le calcul initial

```ts
const five = addCurried(2)(5);
```

Ainsi, une fonction currifiée

- prend un seul argument
- retourne soit une fonction currifiée, soit le résultat attendu

## A quoi ça sert ?

Une fonction curryfiée est une fonction appliquée partiellement dont vous savez à l'avance le fonctionnement

Pas besoin d'avoir à vous préoccuper s'il faut lui donner un, deux ou trois arguments, elle prendra toujours un seul argument et renverra une fonction qui en prendra un aussi

Ainsi, si vous créez une fonction currifiée, vous savez qu'elle est en attente d'un argument pour soit retourner une fonction currifiée, soit qu'elle s'exécute

En informatique, savoir à quoi ressemble quelque chose de manière certaine, c'est pouvoir effectuer des actions très puissante sur cette chose

```ts
const rawIds = ["id-35", "id-53", "id-657"]
const numericPartOfMyIds = rawIds.map(id => id.split("-")[1];
```

Comme je sais à l'avance à quoi ressemblent mes identifiants, je peux facilement en extraire la partie numérique

```ts
const rawIds = ["29437", "%3%4%5%1%2%3", "id-15"]
```

Avec cette convention d'identifiants là, il m'est beaucoup plus compliqué d'extraire la partie numérique

Le même principe s'applique pour les fonctions

Comme une fonction ne retourne qu'une seule valeur, si une autre fonction ne prend qu'un argument, alors je peux facilement enchaîner le résultat d'une fonction pour le fournir en paramètre à une autre fonction

```ts
const increment = (_: number): number => _ + 1;
const isOver = (boundary: number) => (toTest: number): boolean => toTest > boundary;

[-2, 2, 3]
  .map(increment)
  .filter(isOver(0))
```

Les fonctions `increment`, `isOver` et `isOver(0)` sont très faciles à réutiliser dans d'autres contextes

Ce ne serait pas le cas avec une fonction qui prend plus d'un argument, comme il me faudrait une valeur pour les arguments qui ne sont pas le premier

```ts
const add = (a: number, b: number): number => a + b

[-2, 2, 3]
  .map(/* je ne peux pas utiliser "add" directement ici */)
```

## L'autocurryfication

Ecrire manuellement des fonctions unaires pour curryfier une fonction peut être chronophage

```ts
const addFiveNumbers = (a: number) => (b: number) => (c: number) => (d: number) => (e: number): number => a + b + c + d + e
```

Il existe des bibliothèques qui se chargent d'effectuer cette action manuellement, par exemple `Ramda`

```ts
const addFiveNumbers = R.curry(a: number, b: number, c: number, d: number, e: number): number => a + b + c + d + e)
const addTenToNumber = addFiveNumbers(1, 2, 3, 6);
const twelve = addTenToNumber(2);
```

A noter que cette fonction renvoie une nouvelle fonction qui permet l'application partielle de manière très libre

```ts
const addTenToNumber = addFiveNumbers(1, 2)(3, 6);
```

## La curryfication dans d'autres langages

Les langages orientés fonctionnels essaient de reproduire le comportement des fonctions mathématiques

Or, une fonction mathématique ne peut avoir qu'un seul argument

Par conséquent, les fonctions dans certains langages fonctionnels ne peuvent avoir qu'un seul argument

Il est possible de déclarer des fonctions avec plusieurs arguments mais le compilateur curryfiera automatiquement la fonction

```fsharp
let add a b = a + b
```

Cet exemple en `F#` montre une fonction appelée `add` qui prend deux arguments `a` et `b` pour les additionner

Au moment de la compilation, la fonction générée ressemblera à quelque chose comme

```fsharp
let add a =
  let addSubFuction b =
    a + b
  addSubFuction // retourne la fonction interne
```

Il est ensuite possible d'utiliser la fonction ainsi

```fsharp
let inc b = add 1 b // création d'une fonction "add" avec deux arguments
let incOther = add 1 // on peut aussi écrire la déclaration de "incOther" ainsi, le "b" est implicite
let three = add 1 2
let four = inc 3
```

Le même raisonnement s'applique en `Haskell`

```hs
add :: Num a => a -> a -> a
add a b = a + b -- création d'une fonction "add" avec deux arguments

inc :: Num a => a -> a
inc b = add 1 b -- création d'une nouvelle fonction "inc" en appliquant partiellement "add"

inc = add 1 -- on peut aussi écrire la déclaration de "inc" ainsi, le "b" est implicite
```
