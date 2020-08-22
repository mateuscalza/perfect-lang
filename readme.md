# The perfect programming language

## Principles
* Aligned to the current reality
* Highly readable
* Focused on productivity
* Simple and beauty math
* Never repeat yourself
* Goodbye callbacks, hello observables!
* Keep it simple
* Be reactive

## Math with units
```javascript
{ centimeters as cm, meters as m, int } = import('core/units')
{ log } = import('core/console')

// Divider
divider = int(3) // Implicit

// Value
myValue = m(9) + cm(3)  // Default operators handle default units

// Result
myResult = myValue / divider
log(cm(myResult))
```

## GPU
```javascript
{ float } = import('core/gpu/units')
{ multiply as * } = import('core/gpu/math') // Custom operator
{ pi as π } = import('core/math/constants')
{ squaredRoot } = import('core/math')
{ log } = import('core/console')

x = float(123.45678) // Stored on GPU memory
y = squaredRoot(π) // Stored on RAM

result = x * y // Copy y to GPU memory and process on GPU, because * is a GPU operator

log('Result:', result)
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
weigth = %(20)

sumResult = simd(a, b, (aItem, bItem) => aValue + bValue * weigth)

log('Result: ', sumResult)
```

## Observables and filesystem
```javascript
{ lines } = import('core/filesystem')
{ map, filter, log } = import('core/observables')
{ meters } = import('core/units')

lines('./resouces/terrains.csv')
  .pipe(
    // Skip header line
    filter((line, lineIndex) => lineIndex !== 0),
    
    // Map
    map([width, height] => meters(width * height)),
    
    // Log
    log(),
  )
```

## Algebra
```javascript
{ solve, equal as == } = import('core/math/algebra')
{ log } = import('core/console')

results = solve(
    -6x - 4(x - y) == 0, // Undefined vars is a 'unknown' type by default
    4(y - x) - 12z == 0, // * implicit
    y - z == 2,
)

log(results) // Literal object with x, y and z results
```

## Complex (imaginary) numbers
```javascript
{ polar, imaginary as j, degrees } = import('core/units')
{ log } = import('core/console')

results = solve(
  x + 13 * polar(14.5, degrees(45)) == 16j - 27,
)

log(results.x) // Rectangular result
log(polar(results.x)) // Polar result
```

## Logical test
```javascript
{ module as mod } = import('core/math')

if (100 < x < 300) {

} else if (x in [7, 15, 37] or mod(x, 2) == 0) {

} else {

}
```

## Server, markup and switch
```javascript
{ listen, pathMatch, errors } = import('core/http')
{ div, h1, a } = import('core/utils/html')
{ do } = import('core/observables')

listen(8080)
  .pipe(
  
    do({ request, response }) => {
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
        case /\/user\/([0-9]+)/i as result {
          response.text(`Asking for user number ${result.matches[0]}`)

        }

        // Function
        case pathMatch('/books/:number') as result {
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
unit person {
  string(name).required(),
}

operator function add(person(a), person(b)) {
  return [a, b]
}

return {
  person,
  add,
}
```

Import
```javascript
{ person, add as + } = import('local/person')
{ log } = import('core/console')

alfred = person({
  name: 'Alfred',
})
jarvis = person({
  name: 'Jarvis',
})

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
{ int, float, string } = import('core/units')
{ log } = import('core/console')

function greetings(string(lastName).required().minLength(3).maxLength(50), string(pronoun).minLength(3).maxLength(50) = '') {
  log(`Hello ${pronoun} ${lastName}`)
}

greetings('Calza', 'Mr.')
```

## Custom units
```javascript
{ int, float } = import('core/units')
{ log } = import('core/console')

// Custom unit describing literal objects
unit euroOptions {
  float(dollarCurrent) = 1
}

// Custom unit called as a function
unit function euro(int(value) = 0, euroOptions(options)) {
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

unit currencyOptions {
  string(prefix) = '$'
}

// Custom unit called as component
unit function currency(value, currencyOptions(options)) {
  return (
    <span>{`${options.prefix} ${value}`}</span>
  )
}

log(
  <currency prefix='R$'>23.45</currency>
)
```