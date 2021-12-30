## What is Unions?

Unions allow us to define multiple allowed type members by separating each type member with a vertical line character `|`. With a union, we can re-type the program from the previous exercise like this:

```tsx
let ID: string | number;
// ID can be a number
ID = 1;
// or a string
ID = "Hello";

// Unions can be written anywhere a type value is defined,
// including function parameters:

function printID(ID: string | number) {
  console.log(ID);
}
```

## Type Narrowing

Since `ID` from the example above can be a string or number, we may want to perform different logic in the `printID` function’s body that does one thing for strings and another for numbers. To do this, we could implment a _type guard._

```tsx
if (typeof ID === "string") {
  return ID.toLowerCase();
}
```

If we tried to call `ID.toLowerCase()` outside of the string type guard, TypeScript would complain that the `.toLowerCase()` method does not exist on `number` types. This error would occur because `margin` is typed as a `string | number` union.

This concept is called type narrowing. Type narrowing is when TypeScript can figure out what type a variable can be at a given point in our code.

## Inferred Union Return Types

One of the awesome things about TypeScript is that it’s able to infer types in many cases so that we don’t have to manually write them. A great example is a function’s return type. TypeScript will look at the contents of a function and infer which types the function and infer which types the function can return. If there are multiple possible return types, TypeScript will infer the return type as a union.

For instance, take this example, where we call a function named `createUser()`, which might fall:

```tsx
function createUser(): User | string {
  const randomChance = Math.random() >= 0.5;
  if (randomChance) {
    return { id: 1, username: "nikko" };
  } else {
    return "Could not create a user.";
  }
}
```

## Unions and Arrays

Unions are even more powerful when used in combination with arrays. For instance, we can represent time in TypeScript with a number or a string type. If we had a list of dates in both types, we’d need an array that allows for string and number values. Unions are here to help!

To create a union that supports multiple types for an array’s values, wrap the union in parentheses `(string | number)`, then use array notation `[]`.

```tsx
const dateNumber = new Date().getTime(); // returns a number
const dateString = new Date().toString(); // returns a string

const timesList: (string | number)[] = [dateNumber, dateString];
```

Above, the `timesList` variable is typed to allow `string` and `number` types as values in its array. If we try to add a value to `timesList` that is not either type, like a boolean, TypeScript would display an error that `boolean` types are not allowed inside `timesList`.

<aside>
💡 The parentheses are vitally important to type arrays correctly. If we left out the parentheses and wrote `string | number[]`, that type would allow strings ***or*** arrays of only numbers.

</aside>

## Common Key Value Pairs

When we put tpye members in a union, TypeScript will only allow us to use the common methods and properties that all members of the union share. Take this code:

```tsx
const batteryStatus: boolean | number = false;

batteryStatus.toString(); // No TypeScript error
batteryStatus.toFixed(2); // TypeScript error
```

Since `batteryStatus` can be a `boolean` or a `number`, TypeScript will only allow us to call methods that both `number` and `boolean` share. They both share `.toString()`, so we’re good there. But, since only `number` has a `.toFixed()` method, TypeScript will complain if we try to call it.

This rule also applies to type objects that we define. Take this code:

```tsx
type Goose = {
  isPettable: boolean;
  hasFeathers: boolean;
  canThwartAPicnic: boolean;
};

type Moose = {
  isPettable: boolean;
  hasHoofs: boolean;
};

const pettingZooAnimal: Goose | Moose = {
  isPettable: true,
};

console.log(pettingZooAnimal.isPettable); // No TypeScript error
console.log(pettingZooAnimal.hasHoofs); // TypeScript error
```

Like before, since `.isPettable` is on both `Goose` and `Moose` types, TypeScript will allow us to call it. But since `.hasHoofs` is only a property on `Moose`, we can’t call that method on `pettingZooAnimal`. Any properties or methods that are not shared by all of the union’s members won’t be allowed and will produce a TypeScript error.

## Unions with Literal Types

We can use literal types with TypeScript unions. Literal type unions are useful when we want to create distinct states within a program.

For instance, if we were writing the code that controlled stoplights, we might write a program like this:

```tsx
type Color = "green" | "yellow" | "red";

function changeLight(color: Color) {
  // ...
}
```

With the code above, we could ensure that wherever `changeLight()` is called, that it gets passed only allowed stoplight colors. If we tried to call `changeLight('purple')`, TypeScript would complain, since that is not a valid stoplight color.
