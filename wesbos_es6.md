# ES6

## New Variables

Two new Variables **let** and **const**

As a refresher **var** variables:
* could be reassigned
* were function scoped

**let** & **const**:
* scoped to the block instead of the function
* cannot be redeclared inside the same scope

**const**:
* cannot be updated
* objects can still have their attributes changed

*Immediately Invoked Function Expressions* no longer need to protect scope. Can just wrap it everything in a block ({..})

Can use **let** inside for loops as well for better incrementing of values without weird scope issues

Reccomends using const by default and use let if required

## Arrow Functions

3 main befefits:

1. More concise
2. Implicit returns
3. Doesn't rebind *this* when used inside another function

Can replace `function` with just `()`

```javascript
const myFunction = some_array.map(function(param) {
  // do some stuff
})
```

would become

```javascript
const myFunction = some_array.map((param) => {
  // do some stuff
})
```

To do implicit returns we don't need to state `return`. Note that it is on one line with no paranthesis

```javascript
const myFunction      = some_array.map((param) => param * 2)
const myOtherFunction = some_array.map(() => 2)
```

Arrow functions are always *anonymous* functions, ie they cannot be named. Can however stored them in variables.

```javascript
const myStoredFunction = (param) = > { alert(`hello #{param}`) }
myStoredFunction('Charith')
```

Can also return objects implicitly

```javascript
const someone = (params) = > ({ name: params[0], winner: params[1]})
```

Can also do a nice shortcut in objects:

```javascript
{ name: winner, race: race, place: i }
{ name: winner, race, place: i }
```

When using arrow functions, the value of `this` is inheritted from its parent.

In this case `this` is bound to window:
```javascript
  const box = document.querySelector('.box')
  box.addEventListener('click', () => {
    console.log(this);
  })
```

if you *don't* want this, then use a normal function. `this` will always be inheritted from the parent when using arrow function:

```javascript
const box = document.querySelector('.box')
box.addEventListener('click', function() {
  setTimeout(() => {
    this.classList.toggle('opening');
  }, 500)
})
```

Default arguments can be used in functions to set default values if not passed in. Can only pass just a few arguments

```javascript
function calculateBill(total, tax = 0.33, tip = 0.12) {
  // return ...
}

calculateBill(100, undefined, 0.14)
```

Some situations where you don't want to use arrow functions:

* When you don't want `this` to be bound to the parents and want it bound to the function instead
* When you ned a method to be bound to an object
* When you need to add a prototype method
* When you need arguments objects

## Template Strings

We can now use backticks to use template literal

```javascript
const sentence = `My name is ${charith} `
```

Can use any JS inside the `${...}`

Can create multiline strings:

```javascript
const markup = `
  <div>
    <h2>Hello</h2>
    <p class="someclass">This is a snippet of markup</p>
  </div>
`
document.body.innerHTML = markup;
```

Can also use template strings in the code:

```javascript
const markup = `
  <ul>
    ${dogs.map((dog) => `<li>${dog.name}</li>`).join('')}
  </ul>
`
```

Can use a ternary operator to render nothing when undefined:

```javascript
`${someCondition ? `what to say` : ``}`
```

Tag Template provides a way of tagging a template. Here `strings` is everything that isn't a variable (blocks of strings), and `values` is an array of the variables

```javascript
function highlight(strings, ...values) {
    let str = '';
    strings.forEach((string, i) => {
      str += string + (values[i] || '')
    });
    return str;
}

const sentence = highlight`my dogs name is ${name} and I am ${age}`
```

Data Sanitization is very important when displaying information from the user as they may try and insert something malicious with a bit of JS.

Can achieve this with sanitization

```javascript
funciton(sanitize(strings, ...values) {
  // stuff
})
```
## New String Methods

* .startsWith()
  - `'somestring'.startsWith('starts')` => `true`
  - `'somestring'.startsWith('starts', 3)` - start after 3 characters => `true`
* .endsWith()
  - `'somestring'.endsWith('ing')` => `true`
  - `'somestring'.endsWith('ing', 11)` - only take the first 11 characters => `true`
* .includes()
  - `'something.includes('ethi')'` => `true`
* .repeat()
  - `'somestring'.repeat(10)`
  - Can create a left pad function:
    ```javascript
    function leftPad(str, lenght = 20) {
      return `${' '.repeat(length-str.length)${str}}`
    }

    console.log(leftPad('bmw'))
    console.log(leftPad('vw'))
    ```
## Destructuring

Take the properties `first` and `last` from the object person and save them as variables

```javascript
const { first, last } = person;
```

Can be very useful for nested data inside an object

```javascript
const twitter = myPerson.links.social.twitter
const facebook = myPerson.links.social.facebook
// can become
const { twitter, facebook } = myPerson.links.social
//Can also be used for renaming variables
const { twitter: tweet, facebook:fb } = myPerson.links.social
```

Can also be used to set defaults. If the variable can be destructured from the object it will be otherwise use a default value provided

```javascript
var settings = { width: 300, color: 'black' }
// to use ES6 to add in other properties
const settings = { width: 300, color: 'black' }
const { width = 100, height = 100, color = 'blue', fontsize = 25 } = settings;
```
Can also combine all of this into one. This will:

* take the `w` and save it in a variable called `width`
* `h` doesn't exist so it will set it to a default value of 500 in a variable called `height`

```javascript
const { w:width = 400, h:height = 500 } = { w: 800 }
```

Can do destructuring on arrays

```javascript
const details = ['Charith Perera', 123, 'charithperera.com']

const name = details[0]
const id = details[1]

// can become
const [name, id, website] = details
```

Can be used in conjuection with split

```javascript
const data = 'Some, sort, of, sentence'
const [one, two, three] = data.split(',')
```

Can also use rest operator to group the 'rest' of something. The following will group Jack and James in an array called `players`

```javascript
const team = ['Bob', 'Jim', 'Jack', 'James']
const [captain, assistant, ...players]
```

Destructuring can also be used to swap variables

```javascript
let first = 'Charith'
let last = 'Perera'

[first, last] = [last, first]
```

Destructuring for functions can be used by returning an object and destructuring it immediately. Note that in this case ordering does not matter

```javascript
function converter(amount) {
  const converted = {
    USD: amount * 1,
    AUD: amount * 2
  }
  return converted
}

const { USD, AUD } = converter(100)
const { AUD, USD } = converter(100) // order doesn't matter

```

## Iterables & Looping

Different types of For Loops:

```javascript

// Normal for loops work but not the best for syntax and unclear

for(let i = 0;  i < cuts.length; i++) {
  console.log(cuts[i])
}

// forEach

cuts.forEach((cut) => {
  console.log(cut);
  if(cut === 'Brisket') {
    continue
  }
})

// for in - doesn't do well with adding items to prototypes because we're accessing each element based on index

for (const index in cuts) {
  console.log(cuts[index]);
}

// for of

for (const cut of cuts) {
  console.log(cut);
  if(cut === 'brisket') {
    continue;
  }
  console.log(cut)
}

```
Iterating over objects still isn't possible. Can be achieved by using:

```javascript
Object.keys(apple)
```


## Array Methods


New methods `Array.from()` and `Array.of()`. Particularly useful for operating on DOM elements. ie for things that are Array'ish' but not proper arrays

```javascript
  const people = document.querySelectorAll('.people p') // at this stage people is not a normal array
  const names = people.map(person => person.textContent) // so this won't work

  // to make this work we use
  const peopleArray = Array.from(people)
  const names = peopleArray.map(person => person.textContent) // so this won't work

  // can also do this in one line
  const peopleArray = Array.from(people, person => {
    return person.textContent
  })
```
`Array.of` creates an array of stuff


```javascript
Array.of(12,12,3,4) // [12, 12, 3, 4]
```

`Array.find()` and `Array.findIndex()` an be used for searching

```javascript
// asuming posts is a json response
const post = posts.find(post => post.code === 'asdasdas')

//to find index of a particular thing
const postIndex = posts.findIndex(post => post.code === 'asdasd')
```

`Array.some()` and `Array.every()` checks is some or every item meets a specified condition

```javascript
cont ages = [32, 15, 19, 12]
const adultPresent = ages.some(age => age >= 18) // will return true if finds any above 18

const allOldEnough = ages.every(age => age >= 19) // will return true if everyone over 19
```

## Spread Operator

Spread operator takes anything that can be iterated over (arrays, DOM nodes etc). Can 'spread' out an item into an array

```javascript
// take the two arrays and combine them into one
const pizzas = [...featured, 'veg', ...specialty]

const fridayPizzas = [...pizzas]
```

Exercise in use:

```javascript
const heading = document.querySelector('.jump')
heading.innerHTML = sparanWrap(heading.textContent)

const sparanWrap(word) {
  return [...word].map(letter => '<span>${letter}</span>').join('')
}

```

Spreading into functions

```javascript
const inventors = ['Enstien', 'Newton', 'Galileo']
const newInventors = ['Musk', "Jobs"]

inventors.push.apply(inventors, newInventors) // this will work
inventors.push(...newInventors) // this is clearer

const name = ['Charith', 'Perera']

function sayHi(first, last) {
  alert(`Hey there ${first} ${last}`)
}

sayHi(name[0], name[1]) // this will work
sayHi(...name) // this is clearer

```

the `...` can also be used as a `rest param` in 1) a function or 2) destructuring

```javascript
  function convertCurrency(rate, ..amounts) {
    // rate will be 1.45
    // amounts is 10, 23 ,45 ,1
    return amounts.map(amount => amount*rate)
  }


  // in this case rate, tax, and top are saved first
  function convertCurrency(rate, tax, top,  ..amounts) {
    // rate will be 1.45
    // amounts is 10, 23 ,45 ,1
    return amounts.map(amount => amount*rate)
  }

  const amounts = convertCurrency(1.45, 10, 23, 45, 1)
```

```javascript
const runner = ['Charith Perera', 123, 5.5, 5, 3, 6, 35]
const [name, id, ...runs] = runner
// name = Charith Perera, id = 123, runs = [5.5, 5, etc]
```

## Object Literal Upgrades

```javascript
const first = 'snickers'
const last = 'perera'
const age = 2
const breed = 'Shepherd'
const dog = {
  first: first,
  last: last,
  age: age,
  breed: breed,
  pals: ["Hugo", "Sunny"]
}

// this can become

const dog = {
  first,
  last,
  age,
  breed,
  pals: ["Hugo", "Sunny"]
}
```

Shorthand for functions:

```javascript
const modal = {
  create: function(selector) {

  },
  open: function(content) {

  },
  close: function(goodbye) {

  }

  // these can become

  create(selector) {

  },
  open(content) {

  },
  close(goodbye) {

  }
}
```

## Promises

Promises used for ajax work. Something that will happen in the future. JS is asynchronous!

```javascript
const postsPromise = fetch('someurl')

// will only run when data comes back
postsPromise.then(data => {
  console.log(data)
})

// or implicitly and chained
postsPromise
  .then(data => data.json())
  .then(data => { console.log(data) })
  .catch((err) => {
    console.error(err)
  }) // catch any errors
```

You can also build your own promises

```javascript
// your promise will either be successful or fail
const p = new Promise((resolve, reject) => {
  setTimeout(() => {
    // resolve('Charith is cool')
    reject(Error('BIG ERROR'))
  }, 1000)
})

p
  .then(data => {
    console.log(data)
  })
  .catch(err => {
    console.error(err)
  })
```

Chaining promises:


```javascript
const posts = [some stuff]
const authors = [some more stuff]

function getPostById(id) {
  // new promise
  return new Promise(resolve, reject) => {
    setTimeout(() => {
      const post = posts.find(post => post.id === id)
      if (post) {
        resolve(post) // send the post back
      } else {
        reject(Error('No post blah')) // Send back error
      }
    }, 1000)
  }
}

function hydrateAuthor(post) {
  return new Promise((resolve, reject) => {
    const authorDetails = authors.find(person => person.name === post.author)
    if (authorDetails) {
      post.author = authorDetails;
      resolve(post)
    } else {
      reject(Error("Canot find author"))
    }
  })
}

getPostById(2).
  then(post => {
    return hydrateAuthor(post)
  })
  .then(post => {
    console.log(post)
  })
  .catch(err => {
    console.error(err)
  })
```

To handle multiple promises use `Promises.all()`. This will wait until all promises have resolved

```javascript
Promise
  .all([weather, tweets]) // assuming weather and tweets are two defined promises
  .then(responses => {
    const [weather, tweets] = responses
  })
```

## Symbols

Unique identifiers that avoid naming collisions.

```javascript
const charith = Symbol('Charith') // this is just a descriptor for the unique symbol
const person = Symbol('Charith') // not that same as above

const classRoom = {
  [
    'Charith': { grade: 80, gender: 'male' },
    'Shilani': { grade: 80, gender: 'female' },
    'Charith': { grade: 60, gender: 'male'}
  ]
}

// to avoid this name conflict use symbols

const classRoom = {
  [
    [Symbol('Charith')]: { grade: 80, gender: 'male' },
    [Symbol('Shilani')]: { grade: 80, gender: 'female' },
    [Symbol('Charith')]: { grade: 60, gender: 'male'}
  ]
}

// NOTE symbols are NOT enumrable. ie cannot use for(person in Classroom). Instead can do:

const syms = Object.getOwnPropertySymbols(classRoom) //gives us an array of Symbols
const data = syms.map(sym => classRoom[sym]) // use the symbols as property keys
```

## ESLint

```
npm install -g eslint
eslint bad-code.js
```

Has many rules that can be configured

Refer to `airbnb` style guide for their eslint file to use as a base

## JS Modules and npm

```console
mkdir es6modules
cd es6modules
touch app.js
npm init
```

You can now check `package.json` so see references to anything you install

```
npm install slug --save
npm install lodash flickity --save
```

If `node_modules` folder gets deleted you can restore it with `npm init`

some other packages

```
npm install jquery --save
npm install insane --save
```

Next we need to actually import them in `app.js` to use them:

```javascript
import { uniq } from 'lodash'
import insane from 'insane'
import jsonp from 'jsonp'
```

HOWEVER `import` is technically not valid in browsers yet. So we need to use `Webpack` to get it to work!

We also need `babel` to convert es6 code into es5

```
npm install webpack babel-loader babel-core babel-preset-es2015-native-modules --save-dev

touch webpack.config.js
```

We can then require webpack in the config file and create it:

```javascript
const webpack = require('webpack')
const nodeEnv = process.env.NODE_ENV || 'production'

module.exports = {
  devtool: 'source-map',
  entry: {
    filename: './app.js'
  },
  output: {
    filename: '_build/bundle.js'
  },
  module: {
    loaders:[
      test: /\.js$/,
      exclude: /node_modules/,
      loader: 'babel',
      query: {
        presets: ['es2015-native-modules']
      }
    ]
  },
  plugings: [
    // uglify js
    new webpack.optimize.UglifyJsPlugin({
      compress: { warnings: false },
      output: { comments: false },
      sourcemap: true
    })
    // environemtn plugin
  ]
}
```

Can now run the build to create a build file `bundle.js`

```
npm run build
```

Can now jump back into our `App.js` and do stuff:

```javascript
import { uniq } from 'lodash'
import insane from 'insane'
import jsonp from 'jsonp'

const ages = [1, 1, 3, 34]
console.log(ages.unique())
```

Creating your own modules. Needs to be exported.

There are `default` exports and `named` exports

* `default` - export as default so you can import in as any name
* `named` - can only be imported using the specified name

```javascript
// config.js

export const apiKey = 'abc123' // this is how was do named export
export const url = 'hello.com'

const age = 100
const dog = 'Leo'

export { age, dog as cat } // export multiple named at the same trime

// can also export function
export function sayHi(name) {

}

// export default apiKey // default export

```

```javascript
// app.js

import { uniq } from 'lodash'
import insane from 'insane'
import jsonp from 'jsonp'
//import charithIsKey from `./src/config` // any name can be used for default export

// this is how we used named exports. The names must match. can use as to make an alias
import { apiKey as key, url, sayHi, age, cat } from `./src/config`


const ages = [1, 1, 3, 34]
console.log(ages.unique())

console.log(apiKey)
```

More practice:

```javascript
// user.js

import slug from 'slug'
import { url } from './config'
import base64 from 'base-64'

export default function User(name, email, website) {
  return { name, email, website }
}

export function createUrl(name) {
  return `${url}/users/${slug(name)}`
}

export function gravatar(email) {
  const hash = base64.encode(email)
  const photoUrl = 'https://www.gravatar.com/avatar/${hash}'
  return photoUrl
}
```

this will need base-64

```
npm install nase-64 --save
```

Back in `App.js`

```javascript
// app.js

import { uniq } from 'lodash'
import insane from 'insane'
import jsonp from 'jsonp'
import { apiKey as key, url, sayHi, age, cat } from `./src/config`
import User, { createUrl, gravatar } from './src/user'


const charith = new User('Charith Perera', 'charith.p@gmail.com', 'charithperera.com')
const profile = createUrl(charith.name)
const image = gravatar(charith.email)
```

## Classes

Revision of prototypal inheritence

`prototype methods` are inherited by instances


```javascript
function Dog(name, breed) {
  this.name = name
  this.breed = breed
}

// this will get inherited by all instances of Dog
Dog.prototype.bark = function() {
  console.log(`Bark my name is ${this.name}`)
}

const leo = new Dog('Snickers', 'King Charles')
const sunny = new Dog('Sunny', 'Golden Doodle')

// this will overide the method for all instances
Dark.prototype.bark = function() {
  console.log(`Bark Bark hello My name is ${this.name} and i'm ${this.breed}`)
}


// this is also available to all previous instances
Dog.prototype.cuddle = function() {
  console.log('Cuddling')
}

// NOTE bark and cuddle are on the prototype. Not the actual Dog class
```

We can now turn these classes into ES6 classes

```javascript

//const Dog = class { // class expression
class Dog { // class decleration

  //only required method
  constructor(name, breed) {
    this.name = name
    this.breed = breed
  }

  bark() {
    console.log(`Bark Bark hello My name is ${this.name} and i'm ${this.breed}`)
  }

  cuddle() {
    console.log('Cuddling')
  }

  // static methods ONLY work on Dog class directly
  static info() {
    console.log('Dogs are cool')
  }

  // not a method, a getter
  get description() {
    return `${this.name} is a ${this.breed}`
  }

  // not a method, a setter
  set nicknames(value) {
    this.nick = value.trim()
  }

  get nicknames() {
    return this.nick
  }
}

const leo = new Dog('Snickers', 'King Charles')
const sunny = new Dog('Sunny', 'Golden Doodle')

console.log(leo.description)
```

We can also extend ES6 classes

```javascript
class Animal {
  constructor(name) {
    this.name = name
    this.thirst = 100
    this.belly = []

  }

  drink() {
    this.thirst -= 10
  }

  eat(food) {
    this.belly.push(food)
    return this.belly
  }
}

class Dog extends Animal {
  constructor(name, breed) {
    super(name)
    this.breed = breed
  }

  bark() {
    console.log('Bark bark')
  }
}

const rhino = new Animal('Rhiney')
const snickers = new Dog('Snickers', 'Shepherd')

console.log(snickers.bark())
```

You can create custom collections by extending arrays with classess

```javascript
class MovieCollection extends Array {
  construction(name, ...items) {
    super(...items)
    this.name = name
  }

  add(movie) {
    this.push(movie)
  }

  topRated(limit = 10) {
    return this.sort((a, b) => (a.stars > b.stars ? -1 : 1)).slice(0, limit)
  }
}

const movies = new MovieCollection('Chariths movies', {}, {})
```

## Generators

Functions run from top to bottom. Generators however can be start and stopped

```javascript

// this function would need to be called 3 times to get all three values
// note the *
function* listPeople() {
  yield 'Charith'
  yield 'Perera'
  yield 'Asanka'
}

const people = listPeople()

console.log(people.next()) // "Charith" with a done status of false
console.log(people.next()) // "Perera" with a done status of false
console.log(people.next()) // "Asanka" with a done status of true

// could also use a variable
function* listPeople() {
  let i = 0
  yield i
  i++
  yield i
  i++
  yield i
}
```

Can use Generators for Ajax stuff in situations that can lead to 'callback hell' where one thing waits on another and so on

```javascript
function ajax(url) {
  fetch(url).then(data => data.json()).then(data => dataGen.next(data))
}

function* steps() {
  const asanka = yield ajax('something')
  const charith = yield ajax('something')
  const perera = yield ajax('something')
}

// on page load
const dataGen = steps();
dataGen.next() // kick it off
```

Can also use `for of` to loop generators

## Proxies

Allow you to override default behaviour of an operation. Can use it to intercept the calls and do stuff with it

```javascript
const person = { name: 'Charith', age: 100 }
const personProxy = new Proxy(person, {
  // we now have what is called 'traps'
  get(target, name) {
    console.log(`${target} ${name}`)
    return 'Naaah'
  },
  set(target, name, value) {
    if (typeof value === 'string') {
      target[name] = value.trim()
    }
  }
})

personProxy.name = 'Wesley'
```

Another example

```javascript
const phoneHandler = {
  set(target, name, value) {
    target[name] = value.match(/[0-9]/g).join('')
  },
  get(target, name) {
    return target[name].replace(someregestexpression)
  }
}

const phoneNumbers = new Proxy({}, phoneHandler)
```

You can use proxies to intercept calls to method names that may cause issues and issue a warning

```javascript
const safeHandler = {
  set(target, name, values) {
    const likeyKey = Object.keys(target).find(k =< k.toLowerCase)
    // etc etc check for similarity
  }
}

const safety = new Proxy({ id: 100 }, safeHandler)

safety.ID = 200 // note the case of ID
```

## Sets and WeakSets

A unique array with an API for managing it. Not index based, just a list of items that can be looped over.

```javascript
const people = new Set()
people.add('Charith')
people.add('Asanka')
people.add('Perera')

people.size() // 3
people.delete('Asanka')
people.values() // gives a generator that can use .next or a for of loop

const students = new Set(['Charith', 'Asanka', 'Perera'])
const dogs = ['Leo', 'Jackson']
const dogSet = new Set(dogs)
```

WeakSets can only ever contain objects! They also cannot be iterated over using `for of`. The benefit of them is that they clean themselves up and utilises garbage collection to delete anything inside it that isn't used anymore

## Map and Weak Map

Map is very similar to set except it uses a key and a value instead of just a value

```javascript
const dogs = new Map()

dogs.set('Snickers', 3)
dogs.set('Sunny', 2)
dogs.set('Huge', 10)

dogs.forEach((val, key) => console.log(val, key))

for (const [key, val] of dogs) {
  console.log(key, val)
}
```

Using map for MetaData with objects as keys to keep a useful dictionary that is dynamically updated  to keep information about an object

```javascript
const clickCounts = new Map()
const button = document.querySelectorAll('button')

buttons.forEach(button => {
  clickCounts.set(button, 0) //notice the key is the button itself
  button.addEventListener('click', function() {
    const val = clickCOunts.get(this)
    console.log(val)
    clickCounts.set(this, val + 1)
    console.log(clickCounts)
  })
})
```

Weak Map will do better garbage collection than a normal map

```javascript
let dog1 = { name: 'Snickers' }
let dog2 = { name: 'Sunny' }

const strong = new Map()
const weak = new WeakMap()

strong.set(dog1, 'Snickers is the best')
weak.set(dog2, 'Sunny is the 2nd best')

dog1 = null
dog2 = null

// at this point strong is still holding on to dog1 where as weak has removed it
```

## Async Await Flow Control

Native promises review Using `fetch`

```javascript
const res = fetch('someapiendpoint')
console.log(res) // will show us the PROMISE that the data will come back eventually


const res = fetch('someapiendpoint').then(res => {
  conosle.log(res) // this is better but its unusable
})

// fetch does not assume json

const res = fetch('someapiendpoint').then(res => {
  return res.json() //which returns ANOTHER promise
}).then(res => {
  console.log(res) // finally
})

// to catch an error

const res = fetch('someapiendpoint').then(res => {
  return res.json() //which returns ANOTHER promise
}).then(res => {
  console.log(res) // finally
}).catch(err => {
  console.error("ERROR")
  console.error(err)
})

```

Using `getUserMedia` to get webcam

```javascript
const video = document.querySelector('.handsome')

navigator.mediaDevices.getUserMedia({ video: true }).then(mediaStream => {
  console.log(mediaStream)
  video.srcObject = mediaStream // set the vidoe object on the DOM to the stream
  video.load()
  video.play()
}).catch(err => {
  console.error(err) // handle stuff like permission denied
})

```

Custom promises review

```javascript

// this is callback hell
breath(1000, function() {
  breath(1000, function() {
    breath(1000, function() {
      breath(1000, function() {
        breath(1000, function() {

        })
      })
    })
  })
})


// Do this instead

function breathe(amount) {
  return new Promise((resolve, reject), => {
    if (amount < 500) {
      reject('This is too small')
    }
    setTimeout(() => resolve(`Done for ${amount} ms`), amount)
  })
}

breathe(1000).then(res => {
  console.log(res)
  return breathe(500)
}).then(res => {
  console.log(res)
  return breathe(600)
}).then(res => {
  console.log(res)
  return breathe(200)
}).then(res => {
  console.log(res)
  return breathe(500)
}).then(res => {
  console.log(res)
  return breathe(2000)
}).then(res => {
  console.log(res)
  return breathe(300)
}).then(res => {
  console.log(res)
  return breathe(600)
}).catcj(err => {
  console.error(err)
  console.error("ERROR")
})
```

Async + Await

Synchronous - Wait until done with task before moving on
Asynchronous - Move on straight away without waiting for completiong


```javascript
const res = fetch('someapi') // this will not wait until response comes back
console.log(res)

// if we run an alert while this is going it will stop running
// alert is blocking
setInterval(() => console.log(Date.now()), 500)
```

We can use Asych Await to call Asynchronous functions and await values inside the function. Await is built on top of existing promises.

```javascript
//existing promise function
function breathe(amount) {
  return new Promise((resolve, reject), => {
    if (amount < 500) {
      reject('This is too small')
    }
    setTimeout(() => resolve(`Done for ${amount} ms`), amount)
  })
}

//can't do this. need to be inside marked functions as async
await breathe(100)
await breathe(500)
await breathe(5000)

// by putting await in front, it actually resolves or rejeects here instead of just returning a promise
async function go() {
  const res = await breathe(1000)
  console.log(res)
  const res2 = await breathe(600)
  console.log(res)
  const res3 = await breathe(200)
  console.log(res)
}

go() //no chaining of callbacks required
```

To do error handling

```javascript
async function go() {
  try {
    const res = await breathe(1000)
    console.log(res)
    const res2 = await breathe(600)
    console.log(res)
    const res3 = await breathe(200)
    console.log(res)
  } catch (err) {
    console.error('aoofff')
  }
}

// Can also do this using Higher Order Functions
function catchErrors(fn) {
  return function(...args) {
    return fn(...args).catch((err) => {
      console.error(err)
    })
  }
}

async function go() {
  console.log(`Start ${message}`)
  const res = await breathe(1000)
  console.log(res)
  const res2 = await breathe(600)
  console.log(res)
  const res3 = await breathe(200)
  console.log(res)
}

const wrappedFunction = catchErrors(go)
wrappedFunction('Hello')
```

Waiting on multiple promises

```javascript
async function go() {
  const p1 = fetch('someapi1').then(r => r.json())
  const p2 = fetch('someapi2').then(r => r.json())

  const res = await Promise.all([p1, p2]) // won't resolve until both come back
  console.log(res)
}
```

Without using `then`

```javascript
async function go() {
  const p1 = fetch('someapi1')
  const p2 = fetch('someapi2')

  const res = await Promise.all([p1, p2]) // won't resolve until both come back
  const dataPromises = res.map(r => r.json())
  const p1Andp2 = await Promise.all(dataPromises)
  const [p1, p2] =  = await Promise.all(dataPromises) // use destructiring
}
```

## ES7, ES8 & Beyond

Class Properties - able to give a class variables without putting them in the constructor

`padStart` and `padEnd`

```javascript
const strings = ['charith', 'perera', 'asanka', 'charith asanka perera']
const longestString = strings.sort(str => str.length).map(str => str.length)[0]

// align things to the right
strings.forEach(str => console.log(str.padStart(longestString)))
```

Trailing coma in function arguments

```javascript
const names = ['charith', 'perera', 'asanka',]
const names = {
  charith: 'perera',
  asanka: 'perera', //  this is now allowed
}
```

`Object.entries()` and `Object.values()`

```javascript
Object.keys(inventory).map(item => `<li>${item}</li>`)

const totalInventory = Object.values(inventory).reduce(a, b) => a + b)

Object.entries(inventory).forEach(pair => {
  const [key, val] = pair
  console.log(key, val)
})
```
