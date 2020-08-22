# The perfect programming language

## Principles

* Aligned to the current reality
* Highly readable
* Focused on productivity
* Simple and beauty math
* Never repeat yourself
* Keep it simple

## Math with units

```javascript
{ centimeters as cm, meters as m, integer } = import('core/units')
{ log } = import('core/console')

// Value
myValue = 9m + 3cm  // Default operators handle default units

// Divider
divider: integer = 3 // Explicit type

// Result
myResult = myValue / divider
log(cm(myResult))
```

## Multiple units

```
{ millimeter as mm, centimeters as cm, meters as m, cubicMeters as m3, liters, integer } = import('core/units')
{ log } = import('core/console')

width = 120cm
height = 2m
depth = 23mm

boxVolume: m3 = width * height * depth
sphereVolume = 523m3
totalVolume = boxVolume + sphereVolume

log('Capacity', liters(totalVolume))
```

## Symbolic math

```javascript
{ log } = import('core/console')

// No variables, no constants, no undefined. Everything is a symbol
// The transpiler simplify and create a equation for each symbol
// Each provided line, symbols are near to result

-6x - 4 * (x - y) = 0
4*(y - x) - 12z = 0
y - z = 2

log(x) // 1
log(y) // 2.5
log(z) // 0.5
```

## Especial formats
```
{ expression as exp, hexadecimal as hex, binary as bin } = import('core/numbers')
{ regularExpression as reg } = import('core/string')
{ pathMatcher as path } = import('core/http')

hex`3498db`
bin`10001`
reg`[a-z]+`
exp`23 + 3${x} = 33`
path`/books/:id`

```

## SIMD and GPU

```javascript
{ percent as %, vector } = import('core/gpu/units')
{ simd } = import('core/gpu')
{ add as +, multiply as * } = import('core/gpu/math')
{ meters as m } = import('core/units')
{ log } = import('core/console')

a = vector([1, 2, 3, 4])
b = vector([5, 6, 7, 8])
weigth = 20%

result = simd(a, b, (aItem, bItem) => aValue + bValue * weigth)
// With default names, is the same of: sumResult = simd(a, b, (aItem, bItem) => add(aValue, multiply(bValue, weigth)))

log('Result: ', result)
```

## Observables and filesystem
```javascript
{ lines } = import('core/filesystem')
{ map, filter, log } = import('core/observables')
{ squaredMeters as m2 } = import('core/units')

lines('./resouces/terrains.csv')
  .pipe(
    // Skip header line
    filter((line, lineIndex) => lineIndex != 0),
    
    // Map
    map([width, height] => m2(width * height)),
    
    // Log
    log(),
  )
```

## Complex (imaginary) numbers
```javascript
{ polar, imaginary as j, degrees } = import('core/units')
{ log } = import('core/console')

x + 13 * polar(14.5, degrees(45)) = 16j - 27

log(x) // Rectangular result
log(polar(x)) // Polar result
```

## Logical test
```javascript
{ module as mod } = import('core/math')
{ prompt } = import('core/console')

x = int(prompt('x'))

if (100 < x < 300) {

} else if ([7, 15, 37].has(x) or mod(x, 2) == 0) {

} else {

}
```

## Server, markup and switch
```javascript
{ listen, pathMatch as path, errors } = import('core/http')
{ div, h1, a } = import('core/utils/html')
{ do } = import('core/observables')

listen(8080)
  .pipe(
    do(({ request, response }) => {
      switch request.path {
        // Exact string
        case '/' {
          response.html(
            <div>
              <h1>Hello world!</h1>
              <a href='https://github.com/mateuscalza'>Author</a>
            </div>
          )
        }

        // RegExp
        case reg`\/user\/([0-9]+)`.insensitive() as result {
          response.text(`Asking for user number ${result.matches[0]}`)
        }

        // Path
        case path`/books/:number` as result {
          response.text(`Asking for book number ${result.number}`)
        }

        // Others
        default {
          response.error(errors.notFound())
        }
      }
    })

  )
```

## Export, import and custom operator

Export
```javascript
{ string } = import('core/units')

person: unit = {
  name: string
}

add: operator<1> = (a: person, b: person) => [a, b]

return {
  person,
  add,
}
```

Import
```javascript
{ person, add as + } = import('local/person')
{ log } = import('core/console')

alfred: person = {
  name: 'Alfred',
}
jarvis: person = {
  name: 'Jarvis',
}

butlers = alfred + jarvis

log(butlers)
```

## Immutable by design
```javascript
{ vector } = import('core/units')
{ trim, concatenate } = import('core/string')

myVector = vector(['a', 'b', 'c', 'd', 'e', 'f'])

log(myVector.getAt(1)) // 'b'

newVector = myVector.setAt(1, 'g')
log(myVector.getAt(1)) // 'b'
log(newVector.getAt(1)) // 'g'

newVector = myVector
    .setAt(2, 'g')
    .addAt(0, 'm', 'n ')
    .addAtReverse(0, 'y', 'z ')
    .addAt(3, 'j', ' k', 'e')
    .replace('e', 'o')
    .removeAt(1)
    .map(trim)
    .filter(value => value != 'b')
log(newVector) // ['m', 'a', 'j', 'k', 'o', 'g', 'd', 'o', 'f', 'y', 'z']
log(newVector.head(4)) // ['m', 'a', 'j', 'k']
log(newVector.tail(3)) // ['f', 'y', 'z']
log(newVector.reduce(concatenate, '')) // 'majkogdofyz'

```

## Parallel and async/await
```javascript
{ milliseconds as ms } = import('core/units')
{ parallel, delay } = import('core/processing')
{ get } = import('core/requests')
{ log } = import('core/console')

await delay(ms(350))

[a, b] = await parallel(
  async () => await get('https://example.com/a.json'),
  async () => await get('https://example.com/b.json'),
)

log(a + b)
```

## Function and parameter validation
```javascript
{ integer as int, float, string } = import('core/units')
{ log } = import('core/console')

function greetings(string(lastName).required().minLength(3).maxLength(50), string(pronoun).minLength(3).maxLength(50) = '') {
  log(`Hello ${pronoun} ${lastName}`)
}

greetings('Calza', 'Mr.')
```

## Custom units
```javascript
{ integer as int, float } = import('core/units')
{ log } = import('core/console')

// Custom unit describing literal objects
euroOptions: unit = {
  dollarCurrent: float = 1
}

// Custom unit called as a function
euro: unit = (value: int = 0, options: euroOptions) => {
  return {
    value,
    asDollars: () => value * options.dollarCurrent
  }
}

myMoney = euro(123) // Or euro(123, { dollarCurrent: 1 }) // Or <euro dolarCurrent={1}>{123}</euro>
log(myMoney.asDollars())
```

## Custom unit used as HTML
```javascript
{ string } = import('core/units')
{ log } = import('core/console')
{ span } = import('core/utils/html')

currencyOptions: unit = {
  prefix: string = '$'
}

// Custom unit called as component
currency = unit = (value, options: currencyOptions) => {
  return (
    <span>{`${options.prefix} ${value}`}</span>
  )
}

log(
  <currency prefix='R$'>23.45</currency>
)
```
