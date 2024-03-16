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
