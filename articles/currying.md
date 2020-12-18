# Application partielle et curryfication

## Définition du terme *Arité*

Nombre d'arguments dont une fonction a besoin pour être executée

```ts
const addThree = (a: number, b: number, c: number): number => a + b + c
```

Comme la fonction `addThree` a besoin qu'on attribue des valeurs à ses `3` arguments `a`, `b` et `c` pour être executée

```ts
const six = addThree(1, 2, 3);
```

Son arité est donc `3`

## Définition du terme *Unaire*

Fonction dont l'arité est `1`

```ts
const increment = (n: number): number => n + 1;
```

La fonction `increment` a besoin qu'on attribue une valeur à son argument `n` pour être executée

```ts
const two = increment(1);
```

Son arité est donc `1`

Elle est donc dite `unaire`

## Définition du terme *Application partielle*

Produire une nouvelle fonction *B* à partir d'un fonction *A*

L'arité de la fonction *B* doit être inférieure à l'arité de la fonction *A*

```ts
const addThreeNumbers = (a: number, b: number, c: number): number => a + b + c
const addTenToTwoNumbers = (b: number, c: number): number => add(10, b, c);
```

La fonction `addThreeNumbers` a une arité de `3`

La fonction `addTenToTwoNumbers`, créée à partir de `addThreeNumbers`, a une arité de `2`

Il s'agit donc d'application partielle

Le fait de donner une valeur à un argument afin d'effectuer une application partielle s'appelle un `fix` ou un `bind`

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
const urlTointernshipOffer = buildSecureUrl("careers.website.com", "offer?id=10");
```

On utilise ainsi l'application partielle pour éviter de se répéter ou pour éviter d'effectuer des calculs coûteux plusieurs fois

## Définition du terme *Curryfication*

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

Le même principe s'applique pour les fonctions, si je sais que ma fonction est pure et qu'elle prend un seul argument, je peux l'utiliser à tous les endroits qui attendent des fonctions pures et avec un seul argument

```ts
const increment = (_: number): number => _ + 1;
const isOver = (boundary: number) => (toTest: number): boolean => toTest > boundary;

[-2, 2, 3]
  .map(increment)
  .filter(isOver(0))
```

Les fonctions `increment`, `isOver` et `isOver(0)` sont très faciles à réutiliser dans d'autres contextes

Ce ne serait pas le cas avec une fonction qui prend deux 

Sources: 
- https://wiki.haskell.org/Currying
- https://blog.bitsrc.io/understanding-currying-in-javascript-ceb2188c339
