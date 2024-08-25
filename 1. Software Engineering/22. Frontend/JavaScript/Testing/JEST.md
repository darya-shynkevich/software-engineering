If you have a test that often fails when it's run as part of a larger suite, but doesn't fail when you run it alone, it's a good bet that something from a different test is interfering with this one. You can often fix this by clearing some shared state with `beforeEach`. If you're not sure whether some shared state is being modified, you can also try a `beforeEach` that logs data.


```js
test('two plus two is four', () => {  
    expect(2 + 2).toBe(4);  
    expect(2 + 2).not.toBe(5);  
});
```

`toEqual` recursively checks every field of an object or array:

```js
test('object assignment', () => {  
	const data = {one: 1};  
	data['two'] = 2;  
	expect(data).toEqual({one: 1, two: 2});  
});
```

## Truthiness

- `toBeNull` matches only `null`
- `toBeUndefined` matches only `undefined`
- `toBeDefined` is the opposite of `toBeUndefined`
- `toBeTruthy` matches anything that an `if` statement treats as true
- `toBeFalsy` matches anything that an `if` statement treats as false

## Numbers

```js
test('numbers', () => {  
	const value = 2 + 2;  
	expect(value).toBeGreaterThan(3);  
	expect(value).toBeGreaterThanOrEqual(3.5);  
	expect(value).toBeLessThan(5);  
	expect(value).toBeLessThanOrEqual(4.5);  
	  
	// toBe and toEqual are equivalent for numbers  
	expect(value).toBe(4);  
	expect(value).toEqual(4); 

	const value = 0.1 + 0.2;  
	//expect(value).toBe(0.3); This won't work because of rounding error  
	expect(value).toBeCloseTo(0.3); // This works.
});
```

## Strings

```js
test('there is no I in team', () => {  
	expect('team').not.toMatch(/I/);  
});  
  
test('but there is a "stop" in Christoph', () => {  
	expect('Christoph').toMatch(/stop/);  
});
```

## Arrays and iterables

```js
const shoppingList = [  
	'diapers',  
	'kleenex',  
	'trash bags',  
	'paper towels',  
	'milk',  
];  
  
test('the shopping list has milk on it', () => {  
	expect(shoppingList).toContain('milk');  
	expect(new Set(shoppingList)).toContain('milk');  
});
```

## Exceptions

The function that throws an exception needs to be invoked within a wrapping function otherwise the `toThrow` assertion will fail.

```js
function compileAndroidCode() {  
	throw new Error('you are using the wrong JDK!');  
}  
  
test('compiling android goes as expected', () => {  
	expect(() => compileAndroidCode()).toThrow();  
	expect(() => compileAndroidCode()).toThrow(Error);  
	  
	// You can also use a string that must be contained in the error message or a regexp  
	expect(() => compileAndroidCode()).toThrow('you are using the wrong JDK');  
	expect(() => compileAndroidCode()).toThrow(/JDK/);  
	  
	// Or you can match an exact error message using a regexp like below  
	expect(() => compileAndroidCode()).toThrow(/^you are using the wrong JDK$/); // Test fails  
	expect(() => compileAndroidCode()).toThrow(/^you are using the wrong JDK!$/); // Test pass  
});
```

# Testing Asynchronous Code

## Promises

Return a promise from your test, and Jest will wait for that promise to resolve. If the promise is rejected, the test will fail.

```js
test('the data is peanut butter', () => {  
	return fetchData().then(data => {  
		expect(data).toBe('peanut butter');  
	});  
});
```

## Async/Await

```js
test('the data is peanut butter', async () => {  
	const data = await fetchData();  
	expect(data).toBe('peanut butter');  
});  
  
test('the fetch fails with an error', async () => {  
	expect.assertions(1);  
	try {  
		await fetchData();  
	} catch (error) {  
		expect(error).toMatch('error');  
	}  
});
```

Combine `async` and `await` with `.resolves` or `.rejects`

```js
test('the data is peanut butter', async () => {  
	await expect(fetchData()).resolves.toBe('peanut butter');  
});  
  
test('the fetch fails with an error', async () => {  
	await expect(fetchData()).rejects.toMatch('error');  
});
```

If you expect a promise to be rejected, use the `.catch` method. Make sure to add `expect.assertions` to verify that a certain number of assertions are called. Otherwise, a fulfilled promise would not fail the test.

```js
test('the fetch fails with an error', () => {  
	expect.assertions(1);  
	return fetchData().catch(error => expect(error).toMatch('error'));  
});
```

## Callbacks

```js
test('the data is peanut butter', done => {  
	function callback(error, data) {  
		if (error) {  
			done(error);  
			return;  
		}  
		try {  
			expect(data).toBe('peanut butter');  
			done();  
		} catch (error) {  
			done(error); // If `done()` is never called, the test will fail (with timeout error), which is what you want to happen.
		}  
	}  
	  
	fetchData(callback);  
});
```

If the `expect` statement fails, it throws an error and `done()` is not called. If we want to see in the test log why it failed, we have to wrap `expect` in a `try` block and pass the error in the `catch` block to `done`. Otherwise, we end up with an opaque timeout error that doesn't show what value was received by `expect(data)`.

# Setup and Teardown

```js
beforeEach(() => {  
});  
  
afterEach(() => {  
});

beforeAll(() => {  
});  
  
afterAll(() => {  
});
```

The top level `before*` and `after*` hooks apply to every test in a file. The hooks declared inside a `describe` block apply only to the tests within that `describe` block.

```js
describe('test name', () => {  
	// Applies only to tests in this describe block  
	beforeEach(() => {  
	});  
	  
	test('1', () => {  
	});  
	  
	test('2', () => {  
	});  
});
```

> The top-level `beforeEach` is **executed before** the `beforeEach` inside the `describe` block.

Jest executes all describe handlers in a test file _before_ it executes any of the actual tests. This is another reason to do setup and teardown inside `before*` and `after*` handlers rather than inside the `describe` blocks. Once the `describe` blocks are complete, by default Jest runs all the tests serially in the order they were encountered in the collection phase, waiting for each to finish and be tidied up before moving on.

```js
beforeEach(() => console.log('connection setup'));
beforeEach(() => console.log('database setup'));

afterEach(() => console.log('database teardown'));
afterEach(() => console.log('connection teardown'));

test('test 1', () => console.log('test 1'));

describe('extra', () => {
  beforeEach(() => console.log('extra database setup'));
  afterEach(() => console.log('extra database teardown'));

  test('test 2', () => console.log('test 2'));
});

// connection setup
// database setup
// test 1
// database teardown
// connection teardown

// connection setup
// database setup
// extra database setup
// test 2
// extra database teardown
// database teardown
// connection teardown
```

# Mock Functions

```js
const mockCallback = jest.fn(x => 42 + x);

// mockCallback.mock.calls
// mockCallback.mock.results
// mockCallback.mock.contexts
// mockCallback.mock.lastCall
```

All mock functions have this special `.mock` property, which is where data about how the function has been called and what the function returned is kept.

```js
mockCallback.mockReturnValueOnce(10).mockReturnValueOnce('x').mockReturnValue(true);  
  
console.log(mockCallback(), mockCallback(), mockCallback(), mockCallback());  
// > 10, 'x', true, true
```

## Mocking Modules

```js
import axios from 'axios';  
  
class Users {  
	static all() {  
		return axios.get('/users.json').then(resp => resp.data);  
	}  
}

jest.mock('axios');  
  
test('should fetch users', () => {  
	const users = [{name: 'Bob'}];  
	const resp = {data: users};  
	axios.get.mockResolvedValue(resp);  
	  
	// or you could use the following depending on your use case:  
	// axios.get.mockImplementation(() => Promise.resolve(resp))
	return Users.all().then(data => expect(data).toEqual(users));  
});
```

## Mock Implementations

Still, there are cases where it's useful to go beyond the ability to specify return values and full-on replace the implementation of a mock function. This can be done with `jest.fn` or the `mockImplementationOnce` method on mock functions.

```js
const myMockFn = jest
  .fn(() => 'default')
  .mockImplementationOnce(() => 'first call')
  .mockImplementationOnce(() => 'second call');

console.log(myMockFn(), myMockFn(), myMockFn(), myMockFn());
// > 'first call', 'second call', 'default', 'default'
```

## Mock Names

You can optionally provide a name for your mock functions, which will be displayed instead of `'jest.fn()'` in the test error output. Use [`.mockName()`](https://jestjs.io/docs/mock-function-api#mockfnmocknamename) if you want to be able to quickly identify the mock function reporting an error in your test output.





# References:

1. [Testing](1.%20Software%20Engineering/5.%20Testing/_Base.md)
2. ~~[JEST Documentation](https://jestjs.io/docs/asynchronous)~~