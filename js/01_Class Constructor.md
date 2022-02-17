# 01_Class Constructor

In this page, I would like to share how we build objects which are the same class, and introduce the class constructor.

## Intro
If we would like to have some users, the most basic way is to build them one by one.
```js
const user1 = {
  name: "Regis",
  score: 3,
  increment: function () {
    user1.score++;
  },
};
const user2 = {
  name: "Regis",
  score: 3,
  increment: function () {
    user1.score++;
  },
};
```
But the problem is:
1. The work is tedious, as we have to type their properties like "name" and "score" over and over again;
2. This will waste much space in memory, because the same function is being created over and over again;
3. This can be a hassle when we have many users to build;

## Solution 1: Generate objects using function
```js
function userCreator(name, score) {
  const newUser = {};
  newUser.name = name;
  newUser.score = score;
  newUser.increment = function () {
    newUser.score++;
  };
  return newUser;
}
const user1 = userCreator("Regis", 3);
const user2 = userCreator("Ramses", 5);
```
This can solve almost all the problems mentioned above.

## Solution 2: Using Prototype Chain
```js
function userCreator(name, score) {
  const newUser = Object.create(userFunctionStore);
  newUser.name = name;
  newUser.score = score;
  return newUser;
}
const userFunctionStore = {
  increment: function () {
    this.score++;
  },
  login: function () {
    console.log("Logged in!");
  },
};

const user1 = userCreator("Regis", 3);
const user2 = userCreator("Ramses", 5);
```
By using the `Object.create()` method, we can make `newUser` be the son of `userFunctionStore`, so it can have access to all the methods its father has.
But the code is a little bit long.

## Solution 3: Using the `new` keyword
Now we declare properties inside the create function, and then add methods into its prototype
```js
function UserCreator(name, score) {// Pascal Case
  this.name = name;
  this.score = score;
}
userCreator.prototype.increment = function () {
  this.score++;
};
let user1 = new userCreator("Regis", 3);
```
==Here are what the `new` keyword does under the hood:==
1. Create an empty object
2. Assign the ‘this’ keyword to that object. “this: { }”
3. Copy all the properties from the constructor function into the object
4. Create \__proto\__ property in 'this' object, and assign it the address in memory with the prototype property on the object version of the constructor functions
5. Return the object


## Solution 4: The `class` syntactic sugar
```js
class UserCreator {
  constructor(name, score) {
    this.name = name;
    this.score = score;
  }
  increment(){
    this.score++;
  }
  login(){
    console.log("Log in!");
  }
}
const user1 = new UserCreator("Roxy", 0);
```
This is emerging as a new standard, and it feels more like a style of other languages. I like it.


