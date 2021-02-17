























# JD's Questionably Pragmatic Guide To Functional Programming Patterns

# 0: HOFs, Partial Application, Point-Free Programming and Currying











































# HOFs












































# HOFs: What?

HOF == "Higher Order Function"





































# HOFs: Input - What

Take fn as an argument





































# HOFs: Input - Why

Lets you "customize"/"configure" a function at runtime





































# HOFs: Input - Example

```javascript
const R = require('ramda')

const mapValues = (fn, obj) => {
  return R.zipObj(
    R.keys(obj),
    R.map(fn, R.values(obj))
  )
}

// usage
const upcase = (str) => str.toUpperCase()

const obj1 = {
  foo: 'hello',
  bar: 'hi',
  baz: 'boop'
}

mapValues(upcase, obj1)
```





































# HOFs: Output - What

Return a fn as a value from the "parent" fn




































# HOFs: Output - Why

1. So you can dynamically create a function / have a "function builder"

2. So you can delay evaluation of a "full" function




































# HOFs Output: Example

So you can dynamically create a function / have a "function builder"

```javascript
const makeGreeting = (greetingText, fieldToQuery) => {
  return (printFn, person) => {
    const color = person[fieldToQuery]
    const name = person['name']

    printFn(`${greetingText} ${name} - the color of your ${fieldToQuery} is ${color}`)
  }
}

const person = {
  name: 'JD',
  hair: 'brown',
  eyes: 'blue'
}

const hairColorGreeting = makeGreeting('Hi', 'hair')

const eyeColorGreeting = makeGreeting('Hi', 'eyes')

hairColorGreeting(console.log, person)

eyeColorGreeting(console.log, person)
```




































# HOFs Output: Example

So you can delay evaluation of a "full" function


```javascript
const addCannedValue = (cannedValue) => {
  return (newValue) => {
    return cannedValue + newValue
  }
}

// Or more concisely:
const addCannedValue2 = (cannedValue) => (newValue) => cannedValue + newValue

const add42 = addCannedValue(42)

console.log(add42(100))
```



































# Partial Application: What

Partially applying some arguments before the "full" function gets evaluated



That should sound really familiar since we just did it:

const add100 = addCannedValue(100)


































# Partial Application: What, again

Using "output" HOFs + "filling in" some args in one call before filling in more args in subsequent calls

































# Partial Application: Why

Same reason(s) as "output" HOFs:

1. So you can dynamically create a function / have a "function builder"

2. So you can delay evaluation of a "full" function


But why is that _useful_?

































# Partial Application: Why, again

So that you can be more expressive / concise ("point free" programming)

































# Partial Application: Example

```javascript
const R = require('ramda')

const getField = (field) => (person) => person[field]

const jd = {
  name: 'JD',
  hair: 'brown',
  eyes: 'blue'
}

const ellie = {
  name: 'Ellie',
  hair: 'brown',
  eyes: 'brown'
}

const people = [jd, ellie]

const getNames = R.map(getField('name'))

console.log(getNames(people))
```

































# Aside: Point-free programming? What now?

"Points" are variables

"Point-free" means basically programming without _explicitly_ naming variables in the function definition


































# Point-Free Programming: Example

```javascript
const R = require('ramda')

const person = {
  name: 'JD',
  hair: 'brown',
  eyes: 'blue'
}

// Not point-free
const getPersonName = (person) => R.prop('name', person)

console.log(getPersonName(person))


// Point-free

// (Notice: no "person" variable here; it's implicit because R.prop is partially applied to 1 of 2 args)
const getPersonNamePointFree = R.prop('name')

console.log(getPersonNamePointFree(person))
```

































# Currying: What

Turning functions with multiple args into a series of "output" HOFs that _each_ take one arg

































# Currying: Why

To enable partial application + point-free programming

































# Currying: Example

```javascript
// Un-curried

const addThreeNumbers = (x, y, z) => x + y + z

const result = addThreeNumbers(1, 2, 3)

console.log(result)


// Curried
const addThreeNumbersCurried = (x) => (y) => (z) => x + y + z

const resultCurried = addThreeNumbersCurried(1)(2)(3)

console.log(resultCurried)


// Curried + partially applied

const addLastNumber = addThreeNumbersCurried(1)(2)

const resultLast = addLastNumber(42)

console.log(resultLast)
```

































# Wrap-Up

Input HOFs
  What: Take fn as an argument
  Why: Let you "customize"/"configure" a function at runtime


Output HOFs
  What: Return a fn as an argument
  Why: So you can dynamically create a function / have a "function builder" +
      So you can delay evaluation of a "full" function


Partial application
  What: Partially applying some arguments before the "full" function gets evaluated
  Why: Same as Output HOFs + so that you can be more expressive / concise


Point-free programming
  What: Programming without _explicitly_ supplying varaibles
  Why: (Potentially) more expressive code


Currying
  What: Turning functions with multiple args into a series of "output" HOFs that _each_ take one arg
  Why: To enable partial application + point-free programming




































Thanks!











