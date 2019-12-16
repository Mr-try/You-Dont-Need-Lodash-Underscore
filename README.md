## Quick Links

**[Array](#array)**

1. [_.chunk](#_chunk)
1. [_.flattenDeep](#_flattendeep)
1. [_.fromPairs](#_frompairs)

**[Collection*](#collection*)**

*:heavy_exclamation_mark:<b>Important:</b> Note that most native equivalents are array methods,
and will not work with objects. If this functionality is needed and no object method is provided,
then Lodash/Underscore is the better option.*

1. [_.groupBy](#_groupby)
1. [_.keyBy](#_keyBy)
1. [_.minBy and _.maxBy](#_minby-and-_maxby)
1. [_.orderBy](#_sortby-and-_orderby)
1. [_.range](#_range)
1. [_.sortBy](#_sortby-and-_orderby)

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

**[Number](#number)**

1. [_.inRange](#_inRange)
2. [_.random](#_random)

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

### _.flattenDeep

Recursively flattens array.

  ```js
  // Underscore/Lodash
  _.flattenDeep([1, [2, [3, [4]], 5]]);
  // => [1, 2, 3, 4, 5]

  // Native
  const flattenDeep = (arr) => Array.isArray(arr)
    ? arr.reduce( (a, b) => a.concat(flattenDeep(b)) , [])
    : [arr]

  flattenDeep([1, [[2], [3, [4]], 5]])
  // => [1, 2, 3, 4, 5]
  
  // Native(ES2019)
  [1, [2, [3, [4]], 5]].flat(Infinity)
  // => [1, 2, 3, 4, 5]
  
  const flattenDeep = (arr) => arr.flatMap((subArray, index) => Array.isArray(subArray) ? flattenDeep(subArray) : subArray)

  flattenDeep([1, [[2], [3, [4]], 5]])
  // => [1, 2, 3, 4, 5]
  ```

#### Browser Support

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         46.0 ✔          |          ✔          |          16.0 ✔           |        ✖        |        37.0 ✔         |          7.1 ✔          |


#### Browser Support for `Array.prototype.flat()`

| | ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| | :-----------------------------------------: | :---------------------------------: | :---------------------------------------------: | :-------------------------: | :-------------------------------------: | :-----------------------------------------: |
|          |          69 ✔                     |                   ✖                   |                     62 ✔                       |               ✖               |                 56 ✔                   |                   12 ✔                     |


#### Browser Support for `Array.prototype.flatMap()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|          69 ✔           |          ✖          |           62 ✔            |        ✖        |         56 ✔          |          12 ✔           |


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

### _.minBy and _.maxBy

Use Array#reduce for find the maximum or minimum collection item

  ```js
  // Underscore/Lodash
  var data = [{ value: 6 }, { value: 2 }, { value: 4 }]
  var minItem = _.minBy(data, 'value')
  var maxItem = _.maxBy(data, 'value')
  console.log(minItem, maxItem)
  // output: { value: 2 } { value: 6 }

  // Native
  var data = [{ value: 6 }, { value: 2 }, { value: 4 }]
  var minItem = data.reduce(function(a, b) { return a.value <= b.value ? a : b }, {})
  var maxItem = data.reduce(function(a, b) { return a.value >= b.value ? a : b }, {})
  console.log(minItem, maxItem)
  // output: { value: 2 }, { value: 6 }
  ```

Extract a functor and use es2015 for better code

  ```js
  // utils
  const makeSelect = (comparator) => (a, b) => comparator(a, b) ? a : b
  const minByValue = makeSelect((a, b) => a.value <= b.value)
  const maxByValue = makeSelect((a, b) => a.value >= b.value)

  // main logic
  const data = [{ value: 6 }, { value: 2 }, { value: 4 }]
  const minItem = data.reduce(minByValue, {})
  const maxItem = data.reduce(maxByValue, {})

  console.log(minItem, maxItem)
  // output: { value: 2 }, { value: 6 }

  // or also more universal and little slower variant of minBy
  const minBy = (collection, key) => {
    // slower because need to create a lambda function for each call...
    const select = (a, b) => a[key] <= b[key] ? a : b
    return collection.reduce(select, {})
  }

  console.log(minBy(data, 'value'))
  // output: { value: 2 }
  ```

#### Browser Support for `Array.prototype.reduce()`

| | ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] | |
| | :-----------------------------------------: | :---------------------------------: | :---------------------------------------------: | :-------------------------: | :-------------------------------------: | :-----------------------------------------: |
|     |                 ✔                      |                   ✔                   |                     3.0 ✔                     |          9.0 ✔          |              10.5 ✔                 |                  4.0 ✔                   |

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

**[⬆ back to top](#quick-links)**
### _.sample

Gets a random element from `array`.

  ```js
  // Underscore/Lodash
  const array = [0, 1, 2, 3, 4]
  const result = _.sample(array)
  console.log(result)
  // output: 2

  // Native
  const array = [0, 1, 2, 3, 4]
  const sample = arr => {
    const len = arr == null ? 0 : arr.length
    return len ? arr[Math.floor(Math.random() * len)] : undefined
  }

  const result = sample(array)
  console.log(result)
  // output: 2
  ```

#### Browser Support for `Array.prototype.length()` and `Math.random()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|            ✔            |          ✔          |           1.0 ✔           |        ✔        |           ✔           |            ✔            |

**[⬆ back to top](#quick-links)**

### _.sortBy and _.orderBy

Sorts an array of object based on an object key provided by a parameter (note this is more limited than Underscore/Lodash).

  ```js
  const fruits = [
    {name:"banana", amount: 2},
    {name:"apple", amount: 4},
    {name:"pineapple", amount: 2},
    {name:"mango", amount: 1}
  ];

  // Underscore
  _.sortBy(fruits, 'name');
  // => [{name:"apple", amount: 4}, {name:"banana", amount: 2}, {name:"mango", amount: 1}, {name:"pineapple", amount: 2}]

  // Lodash
  _.orderBy(fruits, ['name'],['asc']);
  // => [{name:"apple", amount: 4}, {name:"banana", amount: 2}, {name:"mango", amount: 1}, {name:"pineapple", amount: 2}]

  // Native
  const sortBy = (key) => {
    return (a, b) => (a[key] > b[key]) ? 1 : ((b[key] > a[key]) ? -1 : 0);
  };

  // The native sort modifies the array in place. `_.orderBy` and `_.sortBy` do not, so we use `.concat()` to
  // copy the array, then sort.
  fruits.concat().sort(sortBy("name"));
  // => [{name:"apple", amount: 4}, {name:"banana", amount: 2}, {name:"mango", amount: 1}, {name:"pineapple", amount: 2}]
  ```

#### Browser Support for `Array.prototype.concat()` and `Array.prototype.sort()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|          1.0 ✔          |          ✔          |           1.0 ✔           |      5.5 ✔      |           ✔           |            ✔            |

**[⬆ back to top](#quick-links)**

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

  // Native (ES6 - IE not supported)
  var object = { a: [{ b: { c: 3 } }] };
  var { a: [{ b: { c: result2 = 1 } }] } = object;
  console.log(result2);
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


### _.pick

Creates an object composed of the object properties predicate returns truthy for.

  ```js
  var object = { 'a': 1, 'b': '2', 'c': 3 };

  // Underscore/Lodash
  var result = _.pick(object, ['a', 'c']);
  console.log(result)
  // output: {a: 1, c: 3}

  // Native
  const { a, c } = object;
  const result = { a, c};
  console.log(result);
  // output: {a: 1, c: 3}
  // for an array of this object --> array.map(({a, c}) => ({a, c}));

  // Native
  function pick(object, keys) {
    return keys.reduce((obj, key) => {
       if (object && object.hasOwnProperty(key)) {
          obj[key] = object[key];
       }
       return obj;
     }, {});
  }
  var result = pick(object, ['a', 'c']);
  console.log(result)
  // output: {a: 1, c: 3}
  ```

#### Browser Support

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         38.0 ✔          |          ✔          |          13.0 ✔           |     12.0 ✔      |        25.0 ✔         |          7.1 ✔          |

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

## Reference

* [Mozilla Developer Network](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference)
* [Underscore.js](http://underscorejs.org)
* [Lodash.js](https://lodash.com/docs)

### _.uniq
Removes all duplicates entries from an array.

  ```js
  // Underscore/Lodash
  var result = _.uniq([1, 2, 1, 4, 1, 3]);
  console.log(result)
  // output: [1, 2, 4, 3]

  // Native
  var result = [... new Set([1, 2, 1, 4, 1, 3])]
  console.log(result)
  // output: [1, 2, 4, 3]
  ```

#### Browser Support for `new Set()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|         38.0 ✔          |       ✔ 12.0        |          13.0 ✔           |     11.0 ✔      |        25.0 ✔         |          7.1 ✔          |

**[⬆ back to top](#quick-links)**

## Number

### _.inRange
Checks if n is between start and up to, but not including, end. If end is not specified, it's set to start with start then set to 0. If start is greater than end the params are swapped to support negative ranges.

  ```js
    // Lodash
    _.inRange(3, 2, 4);
    // output: true
    _.inRange(-3, -2, -6);
    // output: true

    //Native
    const inRange = (num, init, final) => {
      if(final === undefined){
        final = init;
        init = 0;
      }
      return (num >= Math.min(init, final) && num < Math.max(init, final));
    }
    
    //Native
    const inRange = (num, a, b=0) => (Math.min(a,b) <= num && num < Math.max(a,b));

    inRange(3, 2, 4);
    // output: true
    inRange(-3, -2, -6);
    // output: true
  ```

  #### Browser Support for `Math.min() and Math.max()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|            ✔            |          ✔          |             ✔             |        ✔        |           ✔           |            ✔            |

**[⬆ back to top](#quick-links)**

  ### _.random
Produces a random number between the inclusive lower and upper bounds. If only one argument is provided a number between 0 and the given number is returned. If floating is true, or either lower or upper are floats, a floating-point number is returned instead of an integer.

  ```js
    // Lodash
    _.random(0, 5);
    // => an integer between 0 and 5
    
    _.random(5);
    // => also an integer between 0 and 5
    
    _.random(5, true);
    // => a floating-point number between 0 and 5
    
    _.random(1.2, 5.2);
    // => a floating-point number between 1.2 and 5.2

    //Native ES6
    const random = (a = 1, b = 0) => {
      const lower = Math.min(a, b);
      const upper = Math.max(a, b);
      return lower + Math.random() * (upper - lower);
    };

    const randomInt = (a = 1, b = 0) => {
      const lower = Math.ceil(Math.min(a, b));
      const upper = Math.floor(Math.max(a, b));
      return Math.floor(lower + Math.random() * (upper - lower + 1))
    };

    random();
    // => a floating-point number between 0 and 1
    
    random(5);
    // => a floating-point number between 0 and 5
    
    random(0, 5);
    // => also a floating-point number between 0 and 5
    
    random(1.2, 5.2);
    // => a floating-point number between 1.2 and 5.2

    randomInt();
    // => just 0 or 1
    
    randomInt(5);
    // => an integer between 0 and 5
    
    randomInt(0, 5);
    // => also an integer between 0 and 5
    
    randomInt(1.2, 5.2);
    // => an integer between 2 and 5

  ```

  #### Browser Support for `Math.random()`

| ![Chrome][chrome-image] | ![Edge][edge-image] | ![Firefox][firefox-image] | ![IE][ie-image] | ![Opera][opera-image] | ![Safari][safari-image] |
| :---------------------: | :-----------------: | :-----------------------: | :-------------: | :-------------------: | :---------------------: |
|            ✔            |          ✔          |             ✔             |        ✔        |           ✔           |            ✔            |

**[⬆ back to top](#quick-links)**

[chrome-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/chrome/chrome_48x48.png
[firefox-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/firefox/firefox_48x48.png
[ie-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/archive/internet-explorer_9-11/internet-explorer_9-11_48x48.png
[opera-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/opera/opera_48x48.png
[safari-image]: https://raw.githubusercontent.com/alrra/browser-logos/master/src/safari/safari_48x48.png
[edge-image]:
https://raw.githubusercontent.com/alrra/browser-logos/master/src/edge/edge_48x48.png
