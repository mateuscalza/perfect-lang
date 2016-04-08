# The perfect programming language

## Principles
* Aligned to the current reality
* Highly readable
* Focused on productivity
* Simple and beauty math
* Never repeat yourself
* Goodbye callbacks, hello streams
* Keep it simple
* Be reactive

## Powerful math
```javascript
Math:class = import 'core/math';
{ console:function } = import 'core/log';

// Multiplier
myMultiplier:int = Math.log(10000, 10) ** 3;

// Value
@Math.unity('cm')
myValue:float = 9.6;

// Result
myResult:float = myMultiplier * myValue;
console(myResult.to('m') + 'meters');
```

## Awesome lists
```javascript
myFirstList:list = (0, 1, 3, 4, 6);
mySecondList:list = (7, 9, 10);

// Concat
myFullList:list = myFirstList + mySecondList; // (0, 1, 3, 4, 6, 7, 9, 10)

// Repeat
myTripleList:list = mySecondList * 3; // (7, 9, 10, 7, 9, 10, 7, 9, 10)

// Item by index
myItem:int = myFirstList[2]; // 1

// Delete item by index (the last item)
delete myFirstList[-1];

// Slice
myPartialList:list = myFirstList[1:5]; // (1, 3, 4, 6)

// Slice with step
myPartialList:list = myFirstList[1:5:2]; // (1, 4)

// Append (add)
mySecondList.append(12);

// Find index
myFullList.find(3); // 2
```

## Reactive programming friendly
```javascript
Math:class = import 'core/math';
Flow:class = import 'core/flow';
{ console:function } = import 'core/log';

stream onlyInnerRange(start, end) {
  then Flow.filter(item => leftX < item < rightX);
}

@Math.unity('s')
myInterval:int = 3; // 3 seconds

launch Flow.interval(myInterval)
then Flow.map(() => Math.random(10, 100, 1))
then onlyInnerRange(5, 10)
then Flow.console()
```

## Logical test
```javascript
if(300 > x > 100) {

} else if (x in (7, 15, 37) or x % 2 === 0) {

} else {

}
```

## Smart switch
```javascript
switch(location.pathname){

  case '/user':
    launch new UserController().index();
    break;

  @Result({ matchesAs: ids:list })
  case /\/user\/([0-9]+)/:
    launch new HomeController().show(ids[0]);
    break;

  default:
    launch new ErrorController().notFound(location.pathname);
    break;

}
```

## Classes with stream methods
Finally a good organization for reactive code, stream based. Using *launch* keyword to start a stream and *then* to continue.
```javascript
Flow:class = import 'core/flow';

class OddCounter {

  private items:list;

  constructor(...items) {
    this.items = items;
  }

  public function add(number:int) {
    this.items.append(number);
  }

  private stream compatible(items:list) {
    launch Flow.from(items);
    then Flow.filter(item => item % 2 === 0);
  }

  private stream count() {
    then Flow.filter(item => item !== null);
    then Flow.count();
  }

  public stream pleaseCount() {
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
mixin SerializableEntity require iEntity {

  public function toJSON() {
    return toJSON(this);
  }

  public function toHash() {
    return toHash(this);
  }

}

mixin EntityCasts require iEntity {

  @Cast(int)
  public function toInt() {
    return this.id;
  }

  @Cast(string)
  public function toString() {
    return "${this.name} (${this.id})";
  }

}

abstract class Entity {
  @Set(private)
  @Delete(false)
  id:int;

  name:string = null;

  constructor(id:int = null, name:string = null) {
    this.id = id;
    this.name = name;
  }
}

{ Date:class } = import 'core/date';
class PersonEntity extends Entity implements iEntity {
  use SerializableEntity;
  use EntityCasts;

  birthday:Date;

  public set birthday(birthday:string|null|Date) {
    this.birthday = birthday is string ? new Date(birthday) : birthday;
  }

}

jack:PersonEntity = new PersonEntity(1, 'Jack');
jack.birthday = '1990-08-01';
jack.toJSON();
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
  export.isNumeric = something === 13;
  export.isWord = something is string and something.lower() === 'thirteen';
  export.isRomanNumber = something is string and something.upper() === 'XIII';
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
    then Flow.filter(item => item !== 2);
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
if(api.aWord === 'ONION'){}
```
