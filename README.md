# Proposal `String.prototype.split` add a third parameter

Proposal to add a third parameter `collect` to the string split method.

## Motivation

In daily development, using split to cut strings is a very common scenario.

Based on the current implementation, it can meet most scenarios. But for some special cases, multiple steps are required to get the desired result.

For example, I have a string `name=hello=world`, and I want to split the string into two strings (representing key and value respectively, that is, the value can tontain `=`).

Based on the current API, we can achieve the requirements through the following code:

```javascript
const [key, ...value] = 'name=hello=world'.split('=')
const realValue = value.join('=') // hello=world
```
The above method works fine when the specific structure of the string is known, but in some cases, the result may not be what you expect.

eg:
```javascript
const [key, ...value] = 'name'.split('=')
const realValue = value.join('=') // ''
```

At this time, the value obtained is an empty string, which does not meet the expected `undefined`. If a fallback is made for this return result in the subsequent logic, such as `const v = realValue ?? null`, then there may be problems with subsequent logical operations.

In order to meet expectations, we have to do additional judgment logic to handle `realValue = value.length ? value.join('=') : undefined`,  which seems a bit cumbersome.

Therefore, I think it would be helpful to provide a third parameter to determine whether the last string returned is the set of remaining characters.

## Core features

Add a third parameter to the `String.prototype.split(sperator[, limit[, collect]])` method.

eg:

```javascript
const str = 'name=hello=world';
const str1 = 'name';

// Before the proposal
const [key, ...value] = str.split('=');
const realValue = value.length ? value.join('=') : undefined; // hello=world

const [key1, ...value1] = str1.split('=');
const realValue1 = value1.length ? value.join('=') : undefined; // undefined

// In the proposal
const [key2, value2] = str.split('=', 2, true)
console.log(value2) // hello=world

const [key3, value3] = str.split('=', 2, true)
console.log(value3) // undefined
```

## Related
- [String.prototype.split](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/String/split)

