# TC39 Proposal Draft: Throwing TypeError for `in` Operator on Regular Arrays

## Title
Restricting `in` Operator Use on Regular Arrays to Prevent Common Misunderstandings

## Authors
@lxe

## Champions
[To be determined]

## Motivation
The `in` operator in JavaScript is widely misunderstood when applied to arrays. Many developers intuitively expect it to check for the presence of a value within an array, similar to how it checks for the presence of a property within an object. This misunderstanding can lead to bugs and confusion, as the operator actually checks for the presence of an index (property) in an array, not a value. There is little practical use for checking whether an array, especially one instantiated with the regular `[]` constructor, contains a certain property key. This proposal aims to align the behavior of the `in` operator on regular arrays more closely with developer expectations, reducing confusion and potential errors.

## Description
The proposal suggests modifying the behavior of the `in` operator such that it throws a `TypeError` when used on arrays instantiated with the regular Array constructor (`[]`). This behavior would mimic the existing behavior when the `in` operator is used on non-object types (primitives), enhancing language consistency.

The restriction would only apply to arrays created directly from the `Array` constructor and not affect classes derived from `Array`. This consideration ensures that custom array-like implementations with specific uses for property checking can maintain their functionality.

## Examples

### Current Behavior
```javascript
// Example demonstrating current behavior with the `in` operator on an array
const fruits = ['apple', 'banana', 'cherry'];

console.log(1 in fruits);
// Expected output: true
// Explanation: This returns true because there is an element at index 1 in the array.

console.log('banana' in fruits);
// Expected output: false
// Explanation: Misleadingly returns false because 'banana' is searched for as a key (index), not a value.

console.log(3 in fruits);
// Expected output: false
// Explanation: Returns false correctly since there is no element at index 3.

// Demonstrating with an array containing numeric values
console.log(1 in [3, 4, 5]);
// Expected output: true
// Explanation: True because there is an index 1, despite the intuitive expectation to check for the presence of the value 1.
```

### Proposed Change
With the proposed change, using the `in` operator on arrays instantiated with the regular Array constructor (`[]`) would throw a `TypeError`, as shown in the revised examples below:

```javascript
const fruits = ['apple', 'banana', 'cherry'];

// The following would throw a TypeError under the proposed change:
console.log(1 in fruits);
// Throws TypeError: Cannot use 'in' operator on a regular array

console.log('banana' in fruits);
// Throws TypeError: Cannot use 'in' operator on a regular array

console.log(3 in fruits);
// Throws TypeError: Cannot use 'in' operator on a regular array

// This also applies to arrays with numeric values
console.log(1 in [3, 4, 5]);
// Throws TypeError: Cannot use 'in' operator on a regular array
```

## Comparison with Primitive Behavior
The proposal aligns the behavior of the `in` operator on regular arrays with its behavior on primitives, where attempting to use the `in` operator throws a `TypeError`.

## FAQ

- **Q: Will this change break existing code?**
  A: This change is targeted to reduce confusion for new and existing code, aligning with developer expectations. It's narrowly scoped to regular arrays to minimize potential breaks in existing codebases.

- **Q: Why not apply this change to all array-like objects?**
  A: Custom array-like objects and classes extending from `Array` may have legitimate uses for property checks that this proposal aims to preserve.

## Alternatives Considered
- **Educating Developers**: Enhancing documentation and linting rules to warn against common misunderstandings.
- **Runtime Checks**: Encouraging the use of `Array.prototype.includes` for value presence checks without altering the `in` operator's behavior.

## Potential Impacts and Compatibility
This proposal may impact existing codebases that rely on the `in` operator for array property checks. A detailed compatibility assessment and migration strategy will be developed as part of the proposal refinement process.

## Next Steps
1. Gather feedback from the JavaScript community and potential champions.
2. Refine the proposal based on input and submit it to the TC39 for consideration.
3. Prepare for presentation and discussion in TC39 meetings.
