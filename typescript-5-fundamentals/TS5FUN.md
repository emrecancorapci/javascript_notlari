# Typescript 5 Fundamentals, v4

## Tuples

### Exceptions

```ts
const numPair: [number, number] = [1, 2];
numPair.length; // 2

numPair.push(3);
numPair.pop();
numPair.pop();
numPair.pop();

numPair.length; // 2 (even though it's empty)

const numPair2: readonly [number, number] = [1, 2];

numPair2.push(3); // Error
numPair2.pop(); // Error
```

## Union and Intersection Types

### Discriminated Unions

- A union type that has a common, singleton type property — the discriminant.
- The discriminant property is used to narrow down the union to a specific member.

```ts
type Circle = { kind: "circle"; radius: number };
type Square = { kind: "square"; sideLength: number };
type Shape = Circle | Square;

function getArea(shape: Shape) {
  if (shape.kind === "circle") {
    return Math.PI * shape.radius ** 2; // Typescript knows that shape is a Circle because of the discriminant property
  } else {
    return shape.sideLength ** 2;
  }
}
```

### Intersection Types

- A type formed by combining multiple types into one.

```ts
type A = { a: number };
type B = { b: string };
type C = { c: number };

type ABC = A & B & C;

const abc: ABC = { a: 1, b: "not javascript", c: 3 };
```

## Interfaces and Type Aliases

### Inheritance in Type Aliases

- Type aliases can be extended or implemented from other types.

```ts
type SpecialDate = Date & { getDescription(): string };

const piDay: SpecialDate =
Object.assign(
  new Date("2021-03-14"),
  { getDescription: () => "Pi Day" }
);

piDay.getDate(); // 14
piDay.getDescription(); // "Pi Day"
```

### Augmenting Existing Types

- You can add properties to existing types using declaration merging.

```ts
interface Array<T> {
  first(): T;
}

Array.prototype.first = function () {
  return this[0];
};

const arr = [1, 2, 3];
arr.first(); // 1
```

> Augmenting existing types is not recommended because it changes types globally.

### Recursive Types

- Types can be recursive.

```ts
type TreeNode<T> = {
  value: T;
  left?: TreeNode<T> ;
  right?: TreeNode<T>;
};

const tree: TreeNode<number> = {
  value: 1,
  left: {
    value: 2,
    left: { value: 4 },
    right: { value: 5 },
  },
  right: {
    value: 3,
    left: { value: 6 },
    right: { value: 7 },
  },
};
```

## Type Query, Callback and Constructable

### `keyof` Operator

- `keyof` operator returns a union type of all the property names of a type.

```ts
type Point = { x: number; y: number };
type PointKey = keyof Point; // "x" | "y"
```

### Type Registry Pattern

- A pattern to add properties to a type from another module.

`registry.ts`

```ts
export interface DataTypeRegistry {
  // Empty by design
}

// & is used for nicer tooltips
export function fetchRecord(
  arg: keyof DataTypeRegistry && string,
  id: number
  ) {}
```

`blog.ts`

```ts

export interface Blog {
  title: string;
  content: string;
  edit: () => void;
}

// Everything in this block will be merged with DataTypeRegistry in registry.ts
declare module `../registry` {
  export interface DataTypeRegistry {
    blog: Blog;
  }
}
```

### Return Types

```ts
interface TwoNumbers {
  (a: number, b: number): number;
}

// or

type TwoNumbers = (a: number, b: number) => number;
```

### Constructable

- A type that represents a constructor function.

```ts
interface Constructable {
  new(value: number): Date;
}

const DateConstructor: Constructable = Date;
const date = new DateConstructor(2021);
```

### Function Overloads

- A function can have multiple signatures.

```ts
function add(a: number, b: number): number;
function add(a: string, b: string): string;
function add(a: number | string, b: number | string): number | string {
  if (typeof a === "number" && typeof b === "number") {
    return a + b;
  } else {
    return a + b;
  }
}
```

### `this` Type

- `this` type represents the type of the object that the function is bound to.

```ts
function clickHandler(this: HTMLButtonElement, event: MouseEvent) {
  this.disabled = true;
}

const button = document.createElement("button");
const handler = clickHandler.bind(button);
handler(new Event("click"));

// or

clickHandler.call(button, new Event("click"));
```
