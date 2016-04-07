# The perfect programming language

## Stream methods
```javascript
import flow from 'verve/flow';

class OddCounter {

  private list:array;

  constructor(...items) {
    this.list = items;
  }

  public function add(number:int) {
    this.list.append(number);
  }

  private stream compatible(items:array) {
    launch flow.from(items);
    then flow.filter(item => item % 2 === 0);
  }

  private stream count() {
    then flow.filter(item => item !== null);
    then flow.count();
  }

  public stream pleaseCount() {
    launch this.compatible(this.list);
    then this.count();
  }

}

OddCounter test = new OddCounter();
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

## Easy for reactive programming
```javascript
stream isInTheBox(leftX, rightX, topY, bottomY) {
  then flow.filter(item => leftX < item.x < rightX);
  then flow.filter(item => topY < item.y < bottomY);
}

launch flow.interval()
then flow.map(() => {
  x: Math.random(10, 100, 1),
  y: Math.random(10, 250, 1)
})
then isInTheBox()
then flow.console()
```
