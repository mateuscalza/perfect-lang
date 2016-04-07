# The perfect programming language

## Principles
* Aligned to the current reality
* Highly readable
* Focused on productivity
* Simple and beauty math
* Never repeat yourself
* Goodbye callbacks, hello streams
* Keep it simple

## Stream methods
```javascript
import flow from 'core/flow';

class OddCounter {

  private items:list;

  constructor(...items) {
    this.items = items;
  }

  public function add(number:int) {
    this.items.append(number);
  }

  private stream compatible(items:list) {
    launch flow.from(items);
    then flow.filter(item => item % 2 === 0);
  }

  private stream count() {
    then flow.filter(item => item !== null);
    then flow.count();
  }

  public stream pleaseCount() {
    launch this.compatible(this.items);
    then this.count();
  }

}

test:OddCounter = new OddCounter();
launch test.pleaseCount(1,2,3,4,6,7,9);
then flow.console();
```

## Smart switch
```javascript
switch(location.pathname){

  case equal '/user':
    launch new UserController().index();
    break;

  case match '/user/([0-9]+)':
    launch new HomeController().show(matches.first());
    break;

  default:
    launch new ErrorController().notFound(location.pathname);
    break;

}
```

## Reactive programming friendly
```javascript
stream onlyInTheBox(leftX, rightX, topY, bottomY) {
  then flow.filter(item => leftX < item.x < rightX);
  then flow.filter(item => topY < item.y < bottomY);
}

launch flow.interval()
then flow.map(() => {
  x: Math.random(10, 100, 1),
  y: Math.random(10, 250, 1)
})
then onlyInTheBox(15, 80, 50, 200)
then flow.console()
```
