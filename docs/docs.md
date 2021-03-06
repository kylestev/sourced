# TOC
   - [entity](#entity)
     - [#digest](#entity-digest)
     - [#enqueue](#entity-enqueue)
     - [#merge](#entity-merge)
     - [#replay](#entity-replay)
     - [#snapshot](#entity-snapshot)
   - [value](#value)
<a name=""></a>
 
<a name="entity"></a>
# entity
<a name="entity-digest"></a>
## #digest
should wrap param object with method matching calling method name and add to array of newEvents.

```js
var param = {
          data: 'data'
        };

      var test = new TestEntity(),
          data = { test: 'data' };

      test.method(data);

      test.newEvents.length.should.equal(1);
      test.newEvents[0].method.should.equal('method');
      test.newEvents[0].data.should.equal(data);
```

<a name="entity-enqueue"></a>
## #enqueue
should enqueue EventEmitter style events by adding them to array of events to emit.

```js
var test = new TestEntity();

      test.enqueue('something.happened', { data: 'data' }, { data2: 'data2' });

      test.should.have.property('eventsToEmit');

      (Array.prototype.slice.call(test.eventsToEmit[0], 0, 1)[0]).should.eql('something.happened');
      (Array.prototype.slice.call(test.eventsToEmit[0], 1))[0].should.have.property('data', 'data');
      (Array.prototype.slice.call(test.eventsToEmit[0], 1))[1].should.have.property('data2', 'data2');
```

<a name="entity-merge"></a>
## #merge
should marge a snapshot into the current object, overwriting any common properties.

```js
var snapshot = {
          property: true,
          property2: true
        };

      var test = new TestEntity();

      test.merge(snapshot);

      test.property.should.equal(true);
      test.property2.should.equal(true);
```

<a name="entity-replay"></a>
## #replay
should throw an entity error with name of model when attempting to replay a method an entity does not implement.

```js
var events = [
        {
          method: 'someMethod',
          data: { some: 'param' }
        }
      ];

      var test = new TestEntity();

      (function () {
        test.replay(events);
      }).should.throw('method \'someMethod\' does not exist on model \'TestEntity\'');
```

should not emit events during replay.

```js
var events = [
        {
          method: 'method',
          data: { some: 'param' }
        }
      ];

      var test = new TestEntity();

      test.on('method-ed', function () {
        throw new Error('should not emit during replay');
      });

      test.replay(events);
```

<a name="entity-snapshot"></a>
## #snapshot
should return object with current state of the entity.

```js
;
      var param = {
        data: 'data'
      };

      var test = new TestEntity(),
          data = { data: 'data' };

      test.method(data);

      var snapshot = test.snapshot();

      snapshot.property2.should.equal(data.data);
      snapshot.snapshotVersion.should.equal(1);
      snapshot.version.should.equal(1);
```

<a name="value"></a>
# value
should be immutable.

```js
var value = Value({
        property: 'value'
      });

      value.should.have.property('property', 'value');

      value.property = 'new value';

      value.should.have.property('property', 'value');
```

