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

## Classes

```ts
class Cube {
  name: string;
  x: number;
  y: number;
  z: number;

  constructor(name:string, x: number, y: number, z: number) {
    this.x = x;
    this.y = y;
    this.z = z;
  }
  scale(n: number) : void {
    this.x *= n;
    this.y *= n;
    this.z *= n;
  }
  volume() : number {
    return this.x * this.y * this.z;
  }
}
```

### Access Modifiers

- `public` is the default access modifier.
- `private` members are only accessible within the class.
- `protected` members are accessible within the class and its subclasses.
- These doesn't affect the generated JavaScript.

```ts
class Cylinder {
  private name: string;
  public r: number;
  private h: number;

  private _volume: Cube.calculateVolume(this.r, this.h);
  public get volume(): number {
    return this._volume;
  }

  constructor(name:string, r: number, h: number) {
    this.name = name;
    this.r = r;
    this.h = h;
  }

  private static calculateVolume(r: number, h: number): number {
    return Math.PI * r ** 2 * h;
  }
}

const cylinder = new Cylinder("Cylinder", 1, 1);
cylinder.name; // Error
cylinder.r; // 1
cylinder.volume; // 3.141592653589793
cylinder.volume = 10; // Error
```

- Javascript now has private fields and methods. They are denoted by a `#` prefix. You can use them in Typescript by setting `target` to `es2022` in `tsconfig.json`.

```ts
class Cylinder {
  ...
  #volume: Cube.calculateVolume(this.r, this.h);
  public get volume(): number {
    return this.#volume;
  }
  ...
}
```

### `readonly` Modifier

- `readonly` members can only be assigned in the constructor. It doesn't affect the generated JavaScript. You can still change the value of a `readonly` member using `Object.assign` or spread operator.

```ts
class Cube {
  readonly name: string;
  readonly x: number;
  readonly y: number;
  readonly z: number;

  constructor(name:string, x: number, y: number, z: number) {
    this.name = name;
    this.x = x;
    this.y = y;
    this.z = z;
  }
}
```

### `override` Modifier

- You can use `override` modifier to explicitly state that a method is overriding a method from the base class.

```ts
class Shape {
  draw() {
    console.log("Drawing a shape");
  }
}

class Circle extends Shape {
  override draw() {
    console.log("Drawing a circle");
  }
}
```

### Parameter Properties

- You can use access modifiers in constructor parameters to create properties.

```ts
class Cube {
  constructor(
    public name: string,
    private x: number,
    private y: number,
    private z: number
  ) {}
}
```

## Type Guards

### Private Field Presence Check (Class Type Guard)

- You can check if a private field is present using `in` operator.

```ts
class User {
  ...
  #id: number;
  
  equals(other: any): other is User {
    if(other && typeof other === "object" && "#id" in other) {
      // other is a User
      return this.#id === other.#id;
    }
    return false;
  }
  ...
}
```

### Custom Type Guards

- You can create custom type guards using classes. Logic of the type guard function have to be created manually.

```ts
class Circle {
  kind: "circle" = "circle";
  radius: number;
  constructor(radius: number) {
    this.radius = radius;
  }
}

class Square {
  kind: "square" = "square";
  sideLength: number;
  constructor(sideLength: number) {
    this.sideLength = sideLength;
  }
}

function isCircle(shape: Circle | Square): shape is Circle {
  return shape.kind === "circle";
}
```

### Custom Type Guards with Assertion Signatures

- You can create custom type guards using classes. Logic of the type guard function have to be created manually.

```ts
class Circle {
  kind: "circle" = "circle";
  radius: number;
  constructor(radius: number) {
    this.radius = radius;
  }
}

function isCircle(shape: Circle | Square): asserts shape is Circle {
  if (shape.kind !== "circle") {
    throw new Error("Not a circle");
  }
}
```

### Narrowing with `switch(true)` (Typescript 5.3)

- You can use `switch(true)` to narrow down the type of a variable.

```ts
function getArea(shape: Circle | Square) {
  switch (true) {
    case shape.kind === "circle":
      return Math.PI * shape.radius ** 2;
    case shape.kind === "square":
      return shape.sideLength ** 2;
  }
}
```
