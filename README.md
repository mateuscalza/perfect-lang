# The perfect programming language

## Principles
* Aligned to the current reality
* Highly readable
* Focused on productivity
* Simple and beauty math
* Never repeat yourself
* Goodbye callbacks, goodbye streams, hello observables!
* Keep it simple
* Be reactive

## Powerful math
```javascript
object math = import 'core/math'
object unit = import 'core/math/unit'
{ function log } = import 'core/console'

// Divider
int divider = 3

// Value
unit.cm myValue = 43 * (9.6 + 3) // Custom types!

// Result
float myResult = myMultiplier / divider
log('Result: ', myResult.to('m') + 'meters')
```

## GPU
```javascript
object gpu = import 'gpu'
object math = import 'gpu/math'
{ function log } = import 'core/console'

gpu.float x = 123.45678
gpu.float y = math.pi

gpu.float result = x gpu.* y // Custom operators!

log('Result: ', result)
```

## SIMD and GPU
```javascript
object gpu = import 'gpu'
object simd = import 'simd'

gpu.vector of gpu.int a = [1, 2, 3, 4]
gpu.vector of gpu.int b = [5, 6, 7, 8]

gpu.vector of gpu.int sumResult = simd(a, b, (aValue, bValue) => aValue gpu.+ bValue)

log('Result: ', sumResult)
```

## Observables
```javascript
{ function readFile } = import 'fs'
{ object logObserver } = import 'core/console'

interface Person {
  int id,
  string name,
  int age,
}

readFile('./resouces/people.csv', readFile.strategies.lineByLine)
  .filter((line, lineIndex) => lineIndex !== 0) // Skip header line
  .map(line => {
    [
      int id,
      string name,
      int age,
    ] = line.split(',')
    return {
      id,
      name,
      age,
    } as Person
  })
  .pipe(logObserver)
```

## Read file
```javascript
{ function readFile } = import 'fs'
{ function log } = import 'core/console'

string fileContent = await readFile( // await on observable stops on the first item and return
  './resouces/text.txt',
  readFile.strategies.full
)

log('Text: ', fileContent)
```

## Immutable lists
```javascript
object immutable = import 'core/immutable'

immutable.list myFirstList = [0, 1, 3, 4, 6]
immutable.list mySecondList = [7, 9, 10]

// Concat
immutable.list myFullList = myFirstList + mySecondList // [0, 1, 3, 4, 6, 7, 9, 10]

// Repeat
immutable.list myTripleList = mySecondList * 3 // [7, 9, 10, 7, 9, 10, 7, 9, 10]

// Item by index
int myItem = myFirstList[2] // 1
// or
int myItem = myFirstList.get(2) // 1

// Slice
immutable.list myPartialList = myFirstList[1:5] // [1, 3, 4, 6]

// Slice with step
immutable.list myPartialList2 = myFirstList[1:5:2] // (1, 4)

// New list without item by index (the last item)
immutable.list listWithoutLastItem = myFirstList.without(-1)

// Append item (add)
immutable.list listWithoutLastItem = mySecondList.append(12)

// Find first index for value
int index = myFullList.find(3) // 2

// Find all indexes
immutable.list indexed = myFullList.findAll(3)

// Set item
// Wrong way
myFullList[4] = 33 // Throws an error
// Right way
immutable.list newFullList = myFullList.set(4, 33)

// Map, filter and reduce
int result = newFullList
  .filter(value => value % 3 == 0 or value % 7 == 0)
  .map(value => value * 2)
  .reduce((acc, value) => acc - value, 100)
```

## Logical test
```javascript
if (100 < x < 300) {

} else if (x in [7, 15, 37] or x % 2 == 0) {

} else {

}
```

## Routing
```javascript
{ function listen, function router, object errors } = import 'http'
{ function stringSwitch } = import 'core/utils/decision'
itemsApi = import '/api/items'

router app = router()
  .on('get', '/items', () => await itemsApi.all())
  .on('post', '/items', (null, body) => await itemsApi.insert(body))
  .on('get', '/items/:id', ({ id }) => await itemsApi.get(id))
  .on('*', '*', ({ id }) => throw errors.notFound())

listen(async request: await app.match(request), 80)
```

## Cluster
```javascript
{ function listen } = import 'http'
function cluster = import 'cluster'

function response = () => 'Hello World!'
function masterResponse = () => 'Hello world from master!'

listen(cluster(isMaster => isMaster ? masterResponse() : response()), 80)
```

## Storage
```javascript
object storage = import 'fs/storage'

int count = await storage.get('count')
await storage.set('count', count + 1)
```

## Math - Matrices
```javascript
object matrix = import 'core/math/matrix'
{ function log } = import 'core/console'

matrix screen = [
  [1, 2, 3],
  [9, 8, 7],
  [5, 1, 5],
]

int determinant = screen.determinant()
log('Determinant of screen: ', determinant)

matrix squaredScreen = screen matrix.** 2
log('Squared screen: ', squaredScreen)
```

## Math - Matrices on GPU
```javascript
object matrix = import 'gpu/math/matrix' // Use GPU math!
{ function log } = import 'core/console'

matrix screenA = list.generate(0, 99, ()
  => list.generate(0, 99, () => math.randomRange(0, 50))
) // 100x100

matrix screenB = list.generate(0, 99, ()
  => list.generate(0, 99, () => math.randomRange(0, 50))
) // 100x100

matrix resultScreen = screenA matrix.+ screenB

log('Screen: ', resultScreen)
```

## Chains
```javascript
{ function objectOrder } = import 'core/utils/ordering'
{ function average, function median } = import 'core/math/statistic'

interface Person {
  string name,
  int age,
}

list of Person people = [
  { id: 1, name: 'Joseph', age: 31, },
  { id: 2, name: 'John', age: 21, },
  { id: 3, name: 'Mariah', age: 38, },
  { id: 4, name: 'Charlotte', age: 22, },
]

float medianOfAges = people
  .value('age')
  .@median()

string text = people
  .filter(person => person < 25)
  .map(person => `${person.name} has ${person.age} years old.`)
  .join('; ')

list of string peopleWithMoreThan23 = people
  .filter(person => person > 23)
  .value('age')
  .@average()

list peopleLessThan23 = people
  .filter(person => person < 23)
  .order(objectOrder('age', 'descending'))
  .select('id', 'name')
```

## Markup
```javascript
object html = import 'markup/html'
{ function listen } = import 'http'

{ function article, function h2 } = html

html content = (
  <article>
    <h2>Just a test!<h2>
  </article>
)

listen(() => content, 80)
```

## Math - Equation
```javascript
{ function equation, function variable } = import 'core/math/algebra'
{ function log } = import 'core/console'

equation solution = equation(() =>
  equation.(
    variable('y') = variable('x') ** 2 + 3
  )
))

// Is the same of
// equation solution = equation(() =>
//   variable('y') equation.= variable('x') equation.** 2 equation.+ 3
// )

list result = solution.runLimited('x', list.generate(0, 99, acc => acc + 1, 0))
log('Set of results: ', result)
```

## Linear system
```javascript
{ function equation, function variable as var } = import 'core/math/algebra'
{ function log } = import 'core/console'

equation solution = equation(() => [
  equation.(
    var('x') - 2 * var('y') - 2 * var('z') = -1
  ),
  equation.(
    var('x') - var('y') + var('z') = -2
  ),
  equation.(
    2 * var('x') + var('y') + 3 * var('z') = 1
  )
]))

object result = solution.solve()
log('X is ', result.x, 'Y is ', result.y, 'Z is', result.z)
```


# Mad science

## Quantum
```javascript
object quantum = import 'quantum'

quantum.qubit value = 0b00
```

# Deprecated

## Smart switch
```javascript
{ request, test } = import 'core/router';

switch(request.pathname){

  case '/user' {
    launch new UserController().index();
    break;
  }
  
  case '/user' {
    launch new UserController().index();
    break;
  }

  @Through(matches => { &id:number = matches[0] })
  case /\/user\/([0-9]+)/ {
    launch new UsersController().show(id);
    break;
  }
  
  @Through(params => { &id:number = params.id })
  case test('/user/{id:number}/edit') {
    launch new UsersController().edit(id);
    break;
  }

  default {
    launch new ErrorController().notFound(location.pathname);
    break;
  }

}
```

## Classes with stream methods
Finally a good organization for reactive code, stream based. Using *launch* keyword to start a stream and *then* to continue.
```javascript
Flow:class = import 'core/flow';

class OddCounter {

  @Private
  items:list;

  constructor(...items) {
    this.items = items;
  }

  add(number:int) {
    this.items.append(number);
  }

  @Private
  stream compatible(items:list) {
    launch Flow.from(items);
    then Flow.filter(item => item % 2 == 0);
  }

  @Private
  stream count() {
    then Flow.filter(item => item != null);
    then Flow.count();
  }

  stream pleaseCount() {
    launch this.compatible(this.items);
    then this.count();
  }

}

itsACounter:OddCounter = new OddCounter(1, 2, 3, 4, 6, 7, 9);
itsACounter.add(15);

launch itsACounter.pleaseCount();
then Flow.console();
```

## Cool strings
```javascript
oneWord:string = 'pretty';
otherWord:string = 'cool';

// Concat
newSentence:string = oneWord + ' ' + otherWord; // pretty cool

// Repeat
repeated:string = otherWord * 3; // coolcoolcool

// Count all characters
oneWord.count(); // 6

// Count occurances
otherWord.count('o'); // 2
oneWord.count(/([aeyiuo])/g); // 2

// Substring
otherWord[1:3]; // oo

// Reverse
otherWord[::-1]; // looc

// For multiline and template strings, use double quotes
myText:string = "Look!
It's ${oneWord} ${otherWord}.";
/*
Look!
It's pretty cool.
*/
```

## Iterators
```javascript
// Iterate list
for(item:int in (3, 5, 7, 7)) {

}

// Iterate range
{ range:function } = import 'core/gen';
for((index:int, item:int) in range(1, 100)) {

}

// Iterate object
myObj:object = {Anton: 25, John: 58, Jack: 19};
for((name:string, age:int) in object){

}

// Repeat while true
aNumber:int = 100;
while (aNumber > 0) {

    aNumber--;
} else {

}

```


## Literal objects
```javascript
ages:object = {
    Anton: 25,
    Jack: 38
};

moreAges:object = {
    John: 58,
    Jack: 19
};

// Deep extending
updatedAges:object = ages + moreAges; // {Anton: 25, John: 58, Jack: 19}

// One by key
ages.Anton; // 25

// One by key in variable
name:string = 'Anton';
ages[name]; // 25

// Set one by key
ages.Vincent = 22;

// Set one by key in variable
name:string = 'Vincent';
ages[name] = 22;

// Removing one by key
delete ages.Jack;

// Removing one by key in variable
name:string = 'Jack';
delete ages[name];
```

## Classes, interfaces, mixins
```javascript

interface iEntity {
  id:int;
  name:string;
}

{ toJSON:function, toHash:function } = import 'core/coding';
@Mixin
@Require(iEntity)
class SerializableEntity {

  toJSON() {
    return toJSON(this);
  }

  toHash() {
    return toHash(this);
  }

}

@Mixin
@Require(iEntity)
class EntityCasts {

  @Cast(int)
  toInt() {
    return this.id;
  }

  @Cast(string)
  toString() {
    return "${this.name} (${this.id})";
  }

}

@Abstract
class Entity {
  @AllowSet(private)
  @AllowDelete(false)
  id:int;

  name:string = null;

  constructor(id:int = null, name:string = null) {
    this.id = id;
    this.name = name;
  }
}

{ Date:class } = import 'core/date';
@Extends(Entity)
@Implements(iEntity)
@Use(SerializableEntity, EntityCasts)
class PersonEntity {

  birthday:Date;

  @Set
  birthday(birthday:string|null|Date) {
    this.birthday = birthday is string ? new Date(birthday) : birthday;
  }

}

jack:PersonEntity = new PersonEntity(1, 'Jack');
jack.birthday = '1990-08-01';
jack.toJSON();

Cache:class = import 'core/data/cache';

@Singleton
class PersonService {
  items:list;

  constructor() { // Called on first time
    this.items = Cache().stock('items').arrayLink();
  }
  
  insert(person:PersonEntity) {
    this.items.append(person);
    return this;
  }
}

PersonService().insert(jack);
```

## Terrific functions
```javascript
// Simple return
function sum(a:float, b:float):float {
  return a + b;
}
result:float = sum(1.5, 11.5);

// Multiple returns uses the *yield* keyword (generator)
function randomNumbers():int {
    { random } = import 'core/math';
    { range } = import 'core/gen';

    for (i in range(6)){
        yield random(1, 40, 1);
    }

    yield random(1, 15, 1);
}

for(item in randomNumbers()){

}

// One return with multiple data
function analyzeThirteen(something):object {
  export.isNumeric = something == 13;
  export.isWord = something is string and something.lower() == 'thirteen';
  export.isRomanNumber = something is string and something.upper() == 'XIII';
  return export;
}

{ isWord:boolean, isNumeric:boolean, isRomanNumber:boolean } = analyzeThirteen('treze');
// or
result:object = analyzeThirteen('treze');
if(result.isNumeric){}
```

## Exporting and importing
```javascript
// Simple return
// my-module.vv
return 'Hello modules!';
// my-app.vv
tellMe:string = import './my-module';
tellMe; // Hello modules!

// Multiple returns uses the *yield* keyword (generator)
// random-number.vv
{ random } = import 'core/math';
{ range } = import 'core/gen';
for (i in range(6)){
    yield random(1, 40, 1);
}
yield random(1, 15, 1);
// my-app.vv
for(item in import './random-number'){

}

// One return with multiple data
// some-api.vv
Flow:class = import 'core/flow';
export.aWord:string = 'TOMATO';
stream export.aStreamPart(){
    then Flow.filter(item => item != 2);
    then Flow.debounce(500);
}
function export.someFunction(){
    return 123;
}
return export;
// my-app.vv
{ someFunction:function, aStreamPart:stream } = import './some-api';
// or
api:object = import './some-api';
if(api.aWord =='ONION'){}
```
