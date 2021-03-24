























# 1: Thinking in Types — Types, Type Constructors, Kinds
























## Types as sets: What?

Thinking of types as sets: a type is a set + that type's values are objects in that set
































## Types as sets: Basic examples



```
Type            |  Values
----------------------------------------------------
Boolean`*`      |  true, false
Number          | ... -2, -1, 0, 1, 2 ...
String          | "a", "b", "hi", "what's up", ...
`Array<Number>` | [1,2], [-45, 1, 100], [1,2,3,4], ...
```


When you say "variable a has type boolean" it's like saying:
"a is in the set of { true, false }"










`*`: because of interoperability w/ JS, TS wants you to use their native lower-case primitive types rather than the uppercase-named constructor functions.

This is a shitshow when trying to teach type-level programming where conventions like "all types start with an uppercase letter" help drive the point home.

I'm just gonna use uppercase for types going forward but, well...you know what I mean. Ugh, TS...





















### Types as sets: Examples

```typescript

const myBool: Boolean =  true

const aStr: String = "hi"

const arr: Array<Number> = [1,2,3]

```

Notice you can't have the _value_ of `[1,2,3]` without fully realizing the type `Array<Number>`.

If we just said `arr: Array` that doesn't give enough info to create _values_ since we don't know what the type of the individual array items are.

We'll come back to this.



































## Types: Why?

Types allow you to put bounds on a set of values.

, ie: they let you rule out unrepresentable states in your program.


If I'm in a language that properly enforces type checking and I create a type `Foobar` with only 3 allowable values then I _know_ when I see `Foobar` that it won't be an arbitrary string or binary or whatever.

Also: in languages that use dot notation for object values or for module functions, types enable value discovery via editor auto completion.






























## Types: Why? Example
```typescript

// TS definitely makes this clunkier than it could be

interface Baz { _tag: "Baz" }
interface Quux { _tag: "Quux" }
interface Fnargh { _tag: "Fnargh" }

type Foobar = Baz | Quux | Fnargh

const baz: Foobar = ({ _tag: "Baz" })
const quux: Foobar = ({ _tag: "Quux" })
const fnargh: Foobar = ({ _tag: "Fnargh" })

const doSomethingWithAFoobar = (foobar: Foobar) => {
  switch (foobar._tag) {
    case baz._tag: { return "was a Baz" }
    case quux._tag: { return "was a Quux" }
    case fnargh._tag: { return "was a Fnargh" }
  }
}

doSomethingWithAFoobar(baz) // -> "was a Baz"

doSomethingWithAFoobar(42) // -> type error
```
```haskell

-- For reference, the same thing in Haskell:

data Foobar = Baz | Quux | Fnargh

doSomethingWithAFoobar Baz    = "was a Baz"
doSomethingWithAFoobar Quux   = "was a Quux"
doSomethingWithAFoobar Fnargh = "was a Fnargh"

doSomethingWithAFoobar Baz -- -> "was a Baz"

doSomethingWithAFoobar 42 -- -> type error























```

## Kinds: What?
A "kind" is like a type but one level more abstract. Think of it as the "type of a type".


...what does that actually mean though?




What is the kind of the type `String` (ie NOT `"a"` but `String`)?


How about the kind of `Array<Number>`?


What about the kind of _just_ `Array` (ie NOT `Array<Number>`)?
  We know we can't make _values_ of `Array` because we don't know what the type of the items are.
































## Types as "functions": an analogy

Think about this for a second:
```typescript


const construct = (containerType) => (...args) => new containerType(...args)

```


Two notable things about it:
1. It's basically just a wrapper function for `new`
2. It's a curried function


So these things are (basically`*`) equivalent:

```typescript
const literalArray = [1,2,3]                      // [1,2,3]

const newArray = new Array(1,2,3)                 // [1,2,3]

const constructedArray = construct(Array)(1,2,3)  // [1,2,3]

```
, ie we get the _value_ `[1,2,3]` with _type_ `Array<Number>` `*`.









`*`: TS isn't really smart enough to infer that `constructedArray` is an `Array<Number>` based on this definition. But it serves my pedagogical purpose so I'm going to pretend it's fine. Don't @ me.


























But what happens if we _partially apply_ the `construct` function, ie just give the first argument as `Array`?
```typescript

const mystery = construct(Array)

```


...we get a _function_ that, when supplied with an arg, gives us a value:

```typescript
mystery(1,2,3) // -> [1,2,3]

```

Let's compare our `create` at the _value_ level with `Array` at the _type_ level:

```typescript
construct(Array)         // partially applied function
Array                    // partially applied "type-level function"


construct(Array)(1,2,3)  // fully-applied function, eg. value `[1,2,3]`
Array<Number>            // fully-applied "type-level function", eg. a fully realized / concrete type
```




























## Types as "functions"

While most of the types we deal with are concrete types, some types are more like "type-level functions".


Think of `Array` again — by itself we can't make _values_ with `Array`  `*`, it's considered "uninhabited".


But when we supply a _type argument_ to `Array`, ie `Array<String>` or `Array<Number>`, we have enough information to make a concrete type.


`Array` and types like it are _type constructors,_ also called "generic types"











`*`: Yeah, yeah...I know you _technically can make values_ with `Array` because it's a constructor function in JS/TS...pretend like constructor functions aren't a thing for a minute and just think about the _type_ of `Array`, not the _function / value constructor_ part of `Array`. Ugh, TS...





























## Types as functions: another analogy


```typescript
42       //a "concrete value" (nothing needs to be "done to it" to manipulate it)

String   // a "concrete type" (nothing needs to be "done to it" to manipulate it)


const x => x + " hi"
  // a "value constructor" that, when given a value, returns another value

Array
  // a "type constructor" that, when given a type, returns another type (in this case a concrete type)
  ```


























## Type Constructors: What?

A type that takes one or more type-level arguments to produce another type, aka a _"generic"_ type
































## Type Constructors: Examples

`Array` is an example of a type constructor that takes one type argument to realize a concrete type, ie:
```typescript

const myArray: Array<string> = ["hello", "there"]

```

`Record` is an example of a type constructor that takes _two_ arguments to realize a concrete type, one for the type of its keys and one for the type of its values, ie:
```typescript

const myRecord: Record<string, number> = { "hello": 42, "there": 15 }

```






















## Type Constructors: Why?

Type constructors / generics are really useful. They let us abstract over types + create "container" types that can work on any other type.


In most day-to-day programming we're used to thinking about containers based on classic data structures like `Array` and `Record`.


But if we squint a little we can see other uses that are "container-like" but _aren't_ based on classical data structures.

For example:
```typescript

const willBeFortyTwo: Promise<number> = Promise.resolve(42)

```

In future lessons we'll come back to this.

































## Kinds, again: What?


A "kind" is like a type but one level more abstract. Think of it as the "type of a type".




























## Kinds: Exercise

Let's think about this from the perspective of "type level functions", aka "type constructors" aka "generics":


What is the kind of type `String`?














//////////////////





  `Type`

























How about the kind of type `Array<Number>`?



















///////////////





  `Type`

























What about the kind of _just_ the type `Array` (NOT `Array<Number>`)?



















///////////////






  `Type` -> `Type`

























What about the kind of `Promise`?



















///////////////







  `Type` -> `Type`























Finally, what about the kind of type `Record` (ie a dictionary/object where keys are of a single type and values are of another type)?



















///////////////








  `Type` -> `Type` -> `Type`

  (because the types of BOTH the keys AND the values need to be specified before you can get a concrete type)

























## Kinds: Why?

Kinds let you talk about types sort of the same way that types let you talk about values.

They enable thinking with types, ie "type level programming", where things like type constructors are easier to reason about and design































## Wrap-Up



## Types

What: (As sets:) A type is a set + that type's values are objects in that set

What: (As "functions":) Types can also be thought of as "functions", especially when looked at through the lens of type constructors + kinds

Why: they allow you to put bounds on a set of values, ie: they let you rule out unrepresentable states in your program.



## Type Constructors

What: A type that takes one or more type-level arguments to produce another type, aka a _"generic"_ type

Why: They let us abstract over types + create "container" types that can contain / work / provide a "computational context" for any other type



## Kinds
What: A "kind" is like a type but one level more abstract. Think of it as the "type of a type".

Why: They enable thinking with types, ie "type level programming", where things like type constructors are easier to reason about and design
































## Thanks!



























