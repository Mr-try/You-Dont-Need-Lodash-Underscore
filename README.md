## Quick Links

**[Array](#array)**

1. [_.chunk](#_chunk)
1. [_.fromPairs](#_frompairs)

**[Collection*](#collection*)**

1. [_.groupBy](#_groupby)
1. [_.keyBy](#_keyBy)

**[Function](#function)**

1. [_.debounce](#_debounce)
1. [_.partial](#_partial)
1. [_.throttle](#_throttle)

**[Lang](#lang)**

1. [_.isEmpty](#_isempty)

**[Object](#object)**

1. [_.extend](#_extend)
1. [_.get](#_get)
1. [_.pick](#_pick)
1. [_.pickBy](#_pickby)
1. [_.toPairs](#_topairs)

## Array

### _.chunk

Creates an array of elements split into groups the length of size.
```js
// Underscore/Lodash
_.chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]

_.chunk(['a', 'b', 'c', 'd'], 3);
// => [['a', 'b', 'c'], ['d']]


// Native

const chunk = (input, size) => {
  return input.reduce((arr, item, idx) => {
    return idx % size === 0
      ? [...arr, [item]]
      : [...arr.slice(0, -1), [...arr.slice(-1)[0], item]];
  }, []);
};

chunk(['a', 'b', 'c', 'd'], 2);
// => [['a', 'b'], ['c', 'd']]

chunk(['a', 'b', 'c', 'd'], 3);
// => [['a', 'b', 'c'], ['d']]

```

#### Browser Support for Spread in array literals

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         46.0 ✔          |       12.0 ✔        |          16.0 ✔           |        ✖        |        37.0 ✔         |          8.0 ✔          |

**[⬆ back to top](#quick-links)**

### _.fromPairs

Returns an object composed from key-value pairs.

  ```js
  // Underscore/Lodash
  _.fromPairs([['a', 1], ['b', 2]]);
  // => { 'a': 1, 'b': 2 }

  // Native
  const fromPairs = function(arr) {
    return arr.reduce(function(accumulator, value) {
      accumulator[value[0]] = value[1];
      return accumulator;
    }, {})
  }

  // Compact form
  const fromPairs = (arr) => arr.reduce((acc, val) => (acc[val[0]] = val[1], acc), {})

  fromPairs([['a', 1], ['b', 2]]);
  // => { 'a': 1, 'b': 2 }
  ```

#### Browser Support for `Array.prototype.reduce()`

| | ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] | |
| | :-----------------------------------------: | :---------------------------------: | :---------------------------------------------: | :-------------------------: | :-------------------------------------: | :-----------------------------------------: |
|     |                 ✔                      |                  ✔                   |                     3.0 ✔                     |          9.0 ✔          |              10.5 ✔                 |                  4.0 ✔                   |

**[⬆ back to top](#quick-links)**

### _.intersection
Returns an array that is the intersection of all the arrays. Each value in the result is present in each of the arrays.

  ```js
  // Underscore/Lodash
  console.log(_.intersection([1, 2, 3], [101, 2, 1, 10], [2, 1]))
  // output: [1, 2]

  // Native
  var arrays = [[1, 2, 3], [101, 2, 1, 10], [2, 1]];
  console.log(arrays.reduce(function(a, b) {
    return a.filter(function(value) {
      return b.includes(value);
    });
  }));
  // output: [1, 2]

  // ES6
  let arrays = [[1, 2, 3], [101, 2, 1, 10], [2, 1]];
  console.log(arrays.reduce((a, b) => a.filter(c => b.includes(c))));
  // output: [1, 2]
  ```

#### Browser Support for `Array.prototype.reduce()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|            ✔            |          ✔          |           3.0 ✔           |      9.0 ✔      |        10.5 ✔         |          4.0 ✔          |

**[⬆ back to top](#quick-links)**

### _.takeRight

Creates a slice of array with n elements taken from the end. 
:heavy_exclamation_mark: Native slice does not behave entirely the same as the `Lodash` method. See example below to understand why.

  ```js
  // Underscore/Lodash
  _.takeRight([1, 2, 3]);
  // => [3]
  
  _.takeRight([1, 2, 3], 2);
  // => [2, 3]
  
  _.takeRight([1, 2, 3], 5);
  // => [1, 2, 3]

  // Native
  [1, 2, 3].slice(-1);
  // => [3]
  
  [1, 2, 3].slice(-2);
  // => [2, 3]

  [1, 2, 3].slice(-5);
  // => [1, 2, 3]

  // Difference in compatibility
  
  // Lodash
  _.takeRight([1, 2, 3], 0);
  // => []

  // Native
  [1, 2, 3].slice(0);
  // => [1, 2, 3]
  ```

#### Browser Support for `Array.prototype.slice()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|          1.0 ✔          |          ✔          |           1.0 ✔           |        ✔        |           ✔           |            ✔            |

**[⬆ back to top](#quick-links)**

## Collection*

*:heavy_exclamation_mark:<b>Important:</b> Note that most native equivalents are array methods,
and will not work with objects. If this functionality is needed and no object method is provided,
then Lodash/Underscore is the better option.*

### _.groupBy

Group items by key.

  ```js
  // Underscore/Lodash
  var grouped = _.groupBy(['one', 'two', 'three'], 'length')
  console.log(grouped)
  // output: {3: ["one", "two"], 5: ["three"]}

  // Native
  var grouped = ['one', 'two', 'three'].reduce((r, v, i, a, k = v.length) => ((r[k] || (r[k] = [])).push(v), r), {})
  console.log(grouped)
  // output: {3: ["one", "two"], 5: ["three"]}
  ```

  ```js
  // Underscore/Lodash
  var grouped = _.groupBy([1.3, 2.1, 2.4], num => Math.floor(num))
  console.log(grouped)
  // output: {1: [1.3], 2: [2.1, 2.4]}

  // Native
  var grouped = [1.3, 2.1, 2.4].reduce((r, v, i, a, k = Math.floor(v)) => ((r[k] || (r[k] = [])).push(v), r), {})
  console.log(grouped)
  // output: {1: [1.3], 2: [2.1, 2.4]}
  ```

#### Browser Support for `Array.prototype.reduce()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|            ✔            |          ✔          |           3.0 ✔           |      9.0 ✔      |        10.5 ✔         |          4.0 ✔          |

**[⬆ back to top](#quick-links)**

### _.keyBy
:heavy_exclamation_mark: `Not in Underscore.js`
Creates an object composed of keys generated from the results of running each element of collection through iteratee.

  ```js
  // Lodash
  console.log(_.keyBy(['a', 'b', 'c']))
  // output: { a: 'a', b: 'b', c: 'c' }
  console.log(_.keyBy([{ id: 'a1', title: 'abc' }, { id: 'b2', title: 'def' }], 'id')
  // output: { a1: { id: 'a1', title: 'abc' }, b2: { id: 'b2', title: 'def' } }
  console.log(_.keyBy({ data: { id: 'a1', title: 'abc' }}, 'id')
  // output: { a1: { id: 'a1', title: 'abc' }}

  // keyBy for array only
  const keyBy = (array, key) => (array || []).reduce((r, x) => ({ ...r, [key ? x[key] : x]: x }), {});

  // Native
  console.log(keyBy(['a', 'b', 'c']))
  // output: { a: 'a', b: 'b', c: 'c' }
  console.log(keyBy([{ id: 'a1', title: 'abc' }, { id: 'b2', title: 'def' }], 'id')
  // output: { a1: { id: 'a1', title: 'abc' }, b2: { id: 'b2', title: 'def' } }
  console.log(keyBy(Object.values({ data: { id: 'a1', title: 'abc' }}), 'id')
  // output: { a1: { id: 'a1', title: 'abc' }}

  // keyBy for array and object
  const collectionKeyBy = (collection, key) => { 
    const c = collection || {};
    return c.isArray() ? keyBy(c, key) : Object.values(keyBy(c, key));
  }
  ```

#### Browser Support for `Array.prototype.reduce()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|            ✔            |       12.0 ✔        |           3.0 ✔           |      9.0 ✔      |        10.5 ✔         |          4.0 ✔          |

**[⬆ back to top](#quick-links)**

### _.range

Creates an array of numbers progressing from start up to.

  ```js
  // Underscore/Lodash
  _.range(4)  // output: [0, 1, 2, 3]
  _.range(-4) // output: [0, -1, -2, -3]
  _.range(1, 5)     // output: [1, 2, 3, 4]
  _.range(0, 20, 5) // output: [0, 5, 10, 15]

  // Native ( solution with Array.from )
  Array.from({length: 4}, (_, i) => i)  // output: [0, 1, 2, 3]
  Array.from({length: 4}, (_, i) => -i) // output: [-0, -1, -2, -3]
  Array.from({length: 4}, (_, i) => i + 1) // output: [1, 2, 3, 4]
  Array.from({length: 4}, (_, i) => i * 5) // output: [0, 5, 10, 15]

  // Native ( solution with keys() and spread )
  [...Array(4).keys()]  // output: [0, 1, 2, 3]
  [...Array(4).keys()].map(k => -k) // output: [-0, -1, -2, -3]
  [...Array(4).keys()].map(k => k + 1)  // output: [1, 2, 3, 4]
  [...Array(4).keys()].map(k => k * 5)  // output: [0, 5, 10, 15]
  ```


#### Browser Support for `Array.from()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         45.0 ✔          |          ✔          |          32.0 ✔           |        ✖        |           ✔           |          9.0 ✔          |

#### Browser Support for keys and spread in Array literals

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         46.0 ✔          |       12.0 ✔        |          16.0 ✔           |        ✖        |        37.0 ✔         |          7.1 ✔          |

## Function

### _.debounce
Create a new function that calls _func_ with _thisArg_ and _args_.

  ```js
 function debounce(func, wait, immediate) {
	var timeout;
	return function() {
		var context = this, args = arguments;
		clearTimeout(timeout);
		timeout = setTimeout(function() {
			timeout = null;
			if (!immediate) func.apply(context, args);
		}, wait);
		if (immediate && !timeout) func.apply(context, args);
	};
}

// Avoid costly calculations while the window size is in flux.
jQuery(window).on('resize', debounce(calculateLayout, 150));

  ```
#### Browser Support for `debounce`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|          7.0 ✔          |          ✔          |           4.0 ✔           |      9.0 ✔      |        11.6 ✔         |          5.1 ✔          |

 **[⬆ back to top](#quick-links)**
 
 
### _.partial
Create a new function that calls _func_ with _args_.

  ```js
  // Lodash
  function greet(greeting, name) {
    return greeting + ' ' + name;
  }
  var sayHelloTo = _.partial(greet, 'Hello');
  var result = sayHelloTo('Jose')
  console.log(result)
  // output: 'Hello Jose'

  // Native
  function greet(greeting, name) {
    return greeting + ' ' + name;
  }
  var sayHelloTo = (...args) => greet('Hello', ...args)
  var result = sayHelloTo('Jose')
  console.log(result)
  // output: 'Hello Jose'
  
  // Native
  const partial = (func, ...boundArgs) => (...remainingArgs) => func(...boundArgs, ...remainingArgs)
  var sayHelloTo = partial(greet, 'Hello');
  var result = sayHelloTo('Jose')
  console.log(result)
  // output: 'Hello Jose'
  ```

#### Browser Support for Spread

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         46.0 ✔          |       12.0 ✔        |          16.0 ✔           |        ✖        |        37.0 ✔         |          8.0 ✔          |

**[⬆ back to top](#quick-links)**


### _.throttle
Create a new function that limits calls to _func_ to once every given timeframe.

  ```js
  function throttle(func, timeFrame) {
    var lastTime = 0;
    return function () {
        var now = new Date();
        if (now - lastTime >= timeFrame) {
            func();
            lastTime = now;
        }
    };
  }

  // Avoid running the same function twice within the specified timeframe.
  jQuery(window).on('resize', throttle(calculateLayout, 150));

  ```
#### Browser Support for `throttle`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|          5.0 ✔          |       12.0 ✔        |           3.0 ✔           |      9.0 ✔      |        10.5 ✔         |          4.0 ✔          |

 **[⬆ back to top](#quick-links)**
 

## Lang

### _.isEmpty

Checks if value is an empty object or collection.
:heavy_exclamation_mark:`Note this is not evaluating a Set or a Map`

  ```js
  // Lodash
  console.log(_.isEmpty(null)
  // output: true
  console.log(_.isEmpty('')
  // output: true
  console.log(_.isEmpty({})
  // output: true
  console.log(_.isEmpty([])
  // output: true
  console.log(_.isEmpty({a: '1'})
  // output: false

  // Native
  const isEmpty = obj => [Object, Array].includes((obj || {}).constructor) && !Object.entries((obj || {})).length;

  console.log(isEmpty(null)
  // output: true
  console.log(isEmpty('')
  // output: true
  console.log(isEmpty({})
  // output: true
  console.log(isEmpty([])
  // output: true
  console.log(isEmpty({a: '1'})
  // output: false
  ```

#### Browser Support for `Array.prototype.includes()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         47.0 ✔          |       14.0 ✔        |          43.0 ✔           |        ✖        |        34.0 ✔         |          9.0 ✔          |

**[⬆ back to top](#quick-links)**






## Object

### _.extend

The method is used to copy the values of all enumerable own and inherited properties from one or more source objects to a target object.

  ```js
  // Underscore
  // Lodash: _.assignIn
  function Foo() {
    this.c = 3;
  }
  function Bar() {
    this.e = 5;
  }
  Foo.prototype.d = 4;
  Bar.prototype.f = 6;
  var result = _.extend({}, new Foo, new Bar);
  console.log(result);
  // output: { 'c': 3, 'd': 4, 'e': 5, 'f': 6 }

  // Native
  function Foo() {
    this.c = 3;
  }
  function Bar() {
    this.e = 5;
  }
  Foo.prototype.d = 4;
  Bar.prototype.f = 6;
  var result = Object.assign({}, new Foo, Foo.prototype, new Bar, Bar.prototype);
  console.log(result);
  // output: { 'c': 3, 'd': 4, 'e': 5, 'f': 6 }

  //Or using a function
  const extend = (target, ...sources) => {
    let source = [];
    sources.forEach(src => {
      source = source.concat([src, Object.getPrototypeOf(src)])
    })
    return Object.assign(target, ...source)
  };
  console.log(extend({}, new Foo, new Bar));
  // output: { 'c': 3, 'd': 4, 'e': 5, 'f': 6 }
  ```

#### Browser Support for `Object.assign()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         45.0 ✔          |          ✔          |          34.0 ✔           |        ✖        |        32.0 ✔         |          9.0 ✔          |

 **[⬆ back to top](#quick-links)**

 ### _.get

Gets the value at path of object.
*Note: If provided path does not exists inside the object js will generate error.*

  ```js
  // Lodash
  var object = { a: [{ b: { c: 3 } }] };
  var result = _.get(object, 'a[0].b.c', 1);
  console.log(result);
  // output: 3
  
  // Native
  const get = (obj, path, defaultValue) => {
    const result = String.prototype.split.call(path, /[,[\].]+?/)
      .filter(Boolean)
      .reduce((res, key) => (res !== null && res !== undefined) ? res[key] : res, obj);
    return (result === undefined || result === obj) ? defaultValue : result;
  }
  
  var object = { a: [{ b: { c: 3 } }] };
  var result = get(object, 'a[0].b.c', 1); 
  // output: 3
  
  ```

#### Browser Support for Object destructing

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         49.0 ✔          |       14.0 ✔        |          41.0 ✔           |        ✖        |        41.0 ✔         |          8.0 ✔          |

  **[⬆ back to top](#quick-links)**

### _.pickBy

Creates an object composed of the object properties predicate returns truthy for.

  ```js
  var object = { 'a': 1, 'b': null, 'c': 3, 'd': false, 'e': undefined };

  // Underscore/Lodash
  var result = _.pickBy(object);
  console.log(result)
  // output: {a: 1, c: 3}

  // Native
  function pickBy(object) {
      const obj = {};
      for (const key in object) {
          if (object[key] !== null && object[key] !== false && object[key] !== undefined) {
              obj[key] = object[key];
          }
      }
      return obj;
  } 
  var result = pickBy(object);
  console.log(result)
  // output: {a: 1, c: 3}
  ```

#### Browser Support

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|            ✔            |          ✔          |             ✔             |      6.0 ✔      |           ✔           |            ✔            |

**[⬆ back to top](#quick-links)**

### _.toPairs

Retrieves all the given object's own enumerable property `[ key, value ]` pairs.

  ```js
  // Underscore - also called _.pairs
  // Lodash - also called _.entries
  var result = _.toPairs({one: 1, two: 2, three: 3})
  console.log(result)
  // output: [["one", 1], ["two", 2], ["three", 3]]

  // Native
  var result2 = Object.entries({one: 1, two: 2, three: 3})
  console.log(result2)
  // output: [["one", 1], ["two", 2], ["three", 3]]
  ```

#### Browser Support for `Object.entries()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         54.0 ✔          |       14.0 ✔        |          47.0 ✔           |        ✖        |        41.0 ✔         |         10.1 ✔          |

**[⬆ back to top](#quick-links)**