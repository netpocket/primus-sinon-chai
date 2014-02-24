# primus-sinon-chai

Provides additional assertions and stubs I use while
testing realtime applications built using [Primus](https://github.com/primus/primus)

## Install

`npm install --save-dev primus-sinon-chai`

## Example

### helper

```js
var psc = require('primus-sinon-chai');
global.expect = psc.chai.expect;
global.sinon = psc.sinon;
global.sparkSpy = psc.spark;
```

### tests

```js
describe("communicate bridge", function() {
  it("listens to all connected devices", function() {
    expect(bConnA.spark).to.listenOn('device:1');
    expect(bConnA.spark).to.listenOn('device:2');
  });

  it("does not listen to itself", function() {
    expect(bConnA.spark).not.to.listenOn('browser:1');
  });

  it("does not listen to other browsers", function() {
    expect(bConnA.spark).not.to.listenOn('browser:2');
  });

  it("works", function() {
    bConnA.spark.onCallback('device:1')({
      listen: "once"
    });

    expect(dConnA.emit).to.have.been.calledWith(
      'relay',
      'browser:1',
      { listen: 'once' }
    );

    dConnA.spark.onceCallback('browser:1')({
      the: 'response'
    });

    expect(bConnA.emit).to.have.been.calledWith(
      'device:1', 
      { the: 'response'}
    );
  });
});
```
