# TypeScript Style Guide and Coding Conventions

---

> Alvys' Official Typescript Style Guide

Key Sections:

- [TypeScript Style Guide and Coding Conventions](#typescript-style-guide-and-coding-conventions)
  - [Variable and Function](#variable-and-function)
  - [Class](#class)
  - [Interface](#interface)
  - [Type](#type)
  - [Namespace](#namespace)
  - [Enum](#enum)
  - [Null vs. Undefined](#null-vs-undefined)
  - [Formatting](#formatting)
  - [Quotes](#quotes)
  - [Spaces](#spaces)
  - [Semicolons](#semicolons)
  - [Array](#array)
  - [Filename](#filename)
  - [type vs. interface](#type-vs-interface)
  - [`==` or `===`](#-or-)

## Variable and Function

---

- Use **`camelCase`** for variable and function names

> Reason: Conventional JavaScript

**Bad**

```ts
var FooVar;
function BarFunc() { }
```

```ts
var foovar;
function foovar() { }
```

**Good**

```ts
var fooVar;
function barFunc() { }
```

## Class

---

- Use `PascalCase` for class names.

> Reason: This is actually fairly conventional in standard JavaScript.

**Bad**

```ts
class foo { }
```

**Good**

```ts
class Foo { }
```

- Use `camelCase` for class members and methods

> Reason: Naturally follows from variable and function naming convention.

**Bad**

```ts
class Foo {
    BarVal: number;
    BazFun() { }
}
```

```ts
class Foo {
    barval: number;
    bazfun() { }
}
```

**Good**

```ts
class Foo {
    barVal: number;
    bazFun() { }
}
```

## Interface

---

- Use `PascalCase` for name.

> Reason: Similar to class

- Use `camelCase` for members.

> Reason: Similar to class

- **Don't** prefix with `I`

> Reason: Unconventional. **`lib.d.ts`** defines important interfaces without an `I` (e.g. Window, Document etc).

**Bad**

```ts
interface IFoo {
}
```

**Good**

```ts
interface Foo {
}
```

## Type

---

- Use `PascalCase` for name.

> Reason: Similar to class

- Use `camelCase` for members.

> Reason: Similar to class

## Namespace

---

- Use `PascalCase` for names

> Reason: Convention followed by the TypeScript team. Namespaces are effectively just a class with static members. Class names are `PascalCase` => Namespace names are `PascalCase`

**Bad**

```ts
namespace foo {
}
```

**Good**

```ts
namespace Foo {
}
```

## Enum

---

- Use `PascalCase` for enum names

> Reason: Similar to Class. Is a Type.

**Bad**

```ts
enum color {
}
```

**Good**

```ts
enum Color {
}
```

- Use `PascalCase` for enum member

> Reason: Convention followed by TypeScript team i.e. the language creators e.g `SyntaxKind.StringLiteral`. Also helps with translation (code generation) of other languages into TypeScript.

**Bad**

```ts
enum Color {
    red
}
```

**Good**

```ts
enum Color {
    Red
}
```

## Null vs. Undefined

- Prefer not to use either for explicit unavailability

> Reason: these values are commonly used to keep a consistent structure between values. In TypeScript you use ***types*** to denote the structure

**Bad**

```ts
let foo = { x: 123, y: undefined };
```

**Good**

```ts
let foo: { x: number, y?: number } = { x:123 };
```

---

- Use `undefined` in general (do consider returning an object like `{valid:boolean, value?:Foo}` instead)

**Bad**

```ts
return null;
```

**Good**

```ts
return undefined;
```

---

- Use `null` where it's a part of the API or conventional

> Reason: It is conventional in Node.js e.g. `error` is `null` for NodeBack style callbacks.

**Bad**

```ts
cb(undefined)
```

**Good**

```ts
cb(null)
```

---

- Use ***truthy*** check for **objects** being `null` or `undefined`

**Bad**

```ts
if (error === null)
```

**Good**

```ts
if (error)
```

---

- Use `== null` / `!= null` (not `===` / `!==`) to check for `null` / `undefined` on primitives as it works for both `null`/`undefined` but not other falsy values (like `''`, `0`, `false`) e.g.

**Bad**

```ts
if (error !== null) // does not rule out undefined
```

**Good**

```ts
if (error != null) // rules out both null and undefined
```

## Formatting

---

The TypeScript compiler ships with a very nice formatting language service. Whatever output it gives by default is good enough to reduce the cognitive overload on the team.

Moreover, your IDE (vscode) already has formatting support built-in.

Examples:

```ts
// Space before type i.e. foo:<space>string
const foo: string = "hello";
```

## Quotes

---

- Prefer single quotes (`'`) unless escaping.

> Double quotes have their use: Allow easier copy paste of objects into JSON. Allows people to use other languages to work without changing their quote character. Allows you to use apostrophes. Double quotes can be used when defining a string variable e.g. **`[style.color]="'black'"`**.

## Spaces

---

- Use tabs, not spaces.

> Reason: Easier to navigate through code.

## Semicolons

---

- Use semicolons.

> Reasons: Explicit semicolons helps language formatting tools give consistent results. Missing ASI (automatic semicolon insertion) can trip new devs e.g. `foo() \n (function(){})` will be a single statement (not two).

## Array

---

- Annotate arrays as `foos: Foo[]` instead of `foos: Array<Foo>`.

> Reasons: It's easier to read. It's used by the TypeScript team. Makes easier to know something is an array as the mind is trained to detect `[]`.

## Filename

---

Name files with `HyphenCase`. E.g. `my-truck.tsx`, `my-control.tsx`, `truck-list.component.ts` etc.

> Reason: Makes name more readable.

## type vs. interface

---

- Use `type` when you *might* need a union or intersection:

```ts
type Foo = number | { someProperty: number }
```

- Use `interface` when you want `extends` or `implements` e.g.

```ts
interface Foo {
  foo: string;
}
interface FooBar extends Foo {
  bar: string;
}
class X implements FooBar {
  foo: string;
  bar: string;
}
```

## `==` or `===`

---

- Use `===` to evade typeof data errors

> Use `==` when typeof data is not important.
