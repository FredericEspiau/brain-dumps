# Curryfication

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

Sources: 
- https://wiki.haskell.org/Currying
- https://blog.bitsrc.io/understanding-currying-in-javascript-ceb2188c339
