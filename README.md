
## **Introduction to TypeScript**
TypeScript is a superset of JavaScript that introduces optional static typing and other powerful features to JavaScript development. Developed by Microsoft, TypeScript enhances code quality and maintainability.

---

### **1. TypeScript vs JavaScript**
| **Aspect**            | **TypeScript**                                     | **JavaScript**                                   |
|------------------------|---------------------------------------------------|-------------------------------------------------|
| **Typing**             | Statically typed (optional).                     | Dynamically typed.                              |
| **Compilation**        | Needs compilation to JS (`.ts` → `.js`).          | Runs directly in environments like browsers.    |
| **Error Checking**     | Detects errors during development.                | Errors often surface at runtime.               |
| **ESNext Features**    | Supports ESNext features + extra like interfaces. | Supports ESNext depending on environment.       |
| **Tooling**            | Excellent tooling with autocomplete and IntelliSense. | Basic tooling; relies on runtime.              |

#### **Example**
**TypeScript**
```typescript
let greeting: string = "Hello, TypeScript!";
// greeting = 42; // Error: Type 'number' is not assignable to type 'string'.
```

**JavaScript**
```javascript
let greeting = "Hello, JavaScript!";
greeting = 42; // No error until runtime.
```

#### **Question**
- *Why might you choose TypeScript over JavaScript for a large-scale project?*

### **2. TypeScript and JavaScript Interoperability**

---

TypeScript is designed to work seamlessly with JavaScript, enabling gradual adoption of TypeScript into JavaScript projects. This interoperability simplifies the process of transitioning from JavaScript to TypeScript while allowing developers to reuse existing JavaScript libraries and codebases.

---

### **Key Concepts of Interoperability**

#### **1. Using JavaScript in TypeScript**
TypeScript can include JavaScript files without modifications, making it easy to integrate existing JS files into a TypeScript project. 

- **`allowJs` Compiler Option**: Enables TypeScript to process `.js` files in addition to `.ts` files.
- **`checkJs` Compiler Option**: Allows TypeScript to perform type checking on JavaScript files.

**Example**
- JavaScript file (`math.js`):
  ```javascript
  function multiply(a, b) {
      return a * b;
  }
  module.exports = multiply;
  ```

- TypeScript file (`main.ts`):
  ```typescript
  import multiply from './math.js'; // Import JavaScript function
  const result: number = multiply(4, 5);
  console.log(result); // Outputs: 20
  ```

- **`tsconfig.json`:**
  ```json
  {
    "compilerOptions": {
      "allowJs": true,        // Include JavaScript files
      "checkJs": false,       // Optional: Set true for type checking in JavaScript
      "esModuleInterop": true // Enable ES module-style imports
    }
  }
  ```

---

#### **2. Using TypeScript in JavaScript Projects**
TypeScript files are compiled into JavaScript, which can then be used in any JavaScript project. 

- The process involves:
  1. Writing `.ts` files.
  2. Compiling them using the `tsc` compiler (`tsc file.ts`).

**Example**
- TypeScript file (`utils.ts`):
  ```typescript
  export function greet(name: string): string {
      return `Hello, ${name}!`;
  }
  ```

- Compile to JavaScript:
  ```bash
  tsc utils.ts
  ```

- Resulting JavaScript file (`utils.js`):
  ```javascript
  "use strict";
  Object.defineProperty(exports, "__esModule", { value: true });
  exports.greet = void 0;
  function greet(name) {
      return `Hello, ${name}!`;
  }
  exports.greet = greet;
  ```

- Use the compiled JavaScript in another file:
  ```javascript
  const { greet } = require('./utils.js');
  console.log(greet("World")); // Outputs: Hello, World!
  ```

---

#### **3. Declaration Files**
When using a JavaScript library without built-in TypeScript support, **declaration files** (`.d.ts`) provide type definitions.

- Install types for popular libraries:
  ```bash
  npm install --save-dev @types/library-name
  ```

**Example**
Using `lodash` in TypeScript:
```typescript
import _ from 'lodash';
const arr = [1, 2, 3, 4];
console.log(_.chunk(arr, 2)); // Outputs: [[1, 2], [3, 4]]
```

Here, the type definitions for `lodash` come from `@types/lodash`.

---

### **Advantages of Interoperability**
1. **Gradual Migration**: Add TypeScript to an existing JavaScript project incrementally.
2. **Leverage Existing Libraries**: Use any JavaScript library in TypeScript without rewriting.
3. **Enhanced Type Safety**: Optionally enable type checking for `.js` files.

---

### **Interview Questions with Answers**

#### **1. How does TypeScript enable interoperability with JavaScript?**
**Answer**:  
TypeScript provides the `allowJs` compiler option to include `.js` files in the project. With the `checkJs` option, TypeScript can perform type checking on these JavaScript files. Additionally, TypeScript supports `.d.ts` declaration files to add type definitions to JavaScript libraries.

---

#### **2. What is the purpose of the `esModuleInterop` option?**
**Answer**:  
The `esModuleInterop` option enables compatibility between TypeScript’s ES module syntax and CommonJS modules. It simplifies importing default exports from JavaScript libraries written for CommonJS.

Example:
Without `esModuleInterop`:
```typescript
import * as lodash from 'lodash';
```
With `esModuleInterop`:
```typescript
import lodash from 'lodash';
```

---

#### **3. What is a `.d.ts` file, and when would you use it?**
**Answer**:  
A `.d.ts` file is a TypeScript declaration file that provides type definitions for JavaScript code or libraries. It is used when a library does not have built-in TypeScript support. Developers can write custom `.d.ts` files or use community-maintained type definitions from `@types`.

Example:
Custom `.d.ts` file for a library (`math.d.ts`):
```typescript
declare module 'math' {
    export function add(a: number, b: number): number;
    export function multiply(a: number, b: number): number;
}
```

---

#### **4. How can you migrate a JavaScript project to TypeScript incrementally?**
**Answer**:  
To migrate incrementally:
1. Enable `allowJs` in `tsconfig.json` to include JavaScript files in the project.
2. Gradually convert `.js` files to `.ts` files, starting with critical files.
3. Use `checkJs` for type-checking JavaScript files before converting them.
4. Use declaration files for libraries without TypeScript support.

### **3. Installation and Configuration**

Installing and configuring TypeScript involves setting up the TypeScript compiler, creating configuration files, and understanding its options to tailor the environment to your project's needs. Here's a detailed explanation with examples and interview questions.

---

### **Step 1: Installing TypeScript**

TypeScript can be installed globally or locally using npm (Node Package Manager).

#### **Global Installation**
This makes the TypeScript compiler (`tsc`) available system-wide:
```bash
npm install -g typescript
```

#### **Local Installation**
Install TypeScript as a project dependency:
```bash
npm install --save-dev typescript
```

#### **Verify Installation**
Check the installed TypeScript version:
```bash
tsc --version
```

---

### **Step 2: Generating the Configuration File**
TypeScript uses a `tsconfig.json` file to store project-specific compiler options.

#### **Generating `tsconfig.json`**
Run the following command to create a basic `tsconfig.json`:
```bash
tsc --init
```

This creates a `tsconfig.json` file in your project root with default settings.

---

### **Step 3: Understanding `tsconfig.json`**

The `tsconfig.json` file is the core configuration file for a TypeScript project. It allows you to define:
1. Compiler options.
2. Files to include and exclude.
3. Type checking rules.

#### **Basic Structure**
```json
{
  "compilerOptions": {
    "target": "ES6",            // ECMAScript version for output
    "module": "commonjs",       // Module system (e.g., ES6, CommonJS)
    "strict": true,             // Enable strict type checking
    "outDir": "./dist",         // Output directory for compiled files
    "rootDir": "./src",         // Root directory for source files
    "esModuleInterop": true     // Interoperability with CommonJS modules
  },
  "include": ["src/**/*"],       // Files or directories to include
  "exclude": ["node_modules"]    // Files or directories to exclude
}
```

---

### **Step 4: Key Compiler Options**

| **Option**                | **Description**                                                                                   | **Example**                              |
|---------------------------|---------------------------------------------------------------------------------------------------|------------------------------------------|
| **`target`**              | Specifies the JavaScript version to compile into.                                                | `"target": "ES6"`                        |
| **`module`**              | Specifies the module system (`CommonJS`, `ES6`, etc.).                                           | `"module": "commonjs"`                   |
| **`strict`**              | Enables all strict type-checking options (e.g., `noImplicitAny`, `strictNullChecks`).            | `"strict": true`                         |
| **`outDir`**              | Specifies the output directory for compiled JavaScript files.                                    | `"outDir": "./dist"`                     |
| **`rootDir`**             | Specifies the root directory of the TypeScript source files.                                     | `"rootDir": "./src"`                     |
| **`allowJs`**             | Allows JavaScript files in the project.                                                          | `"allowJs": true`                        |
| **`checkJs`**             | Enables type-checking for JavaScript files.                                                      | `"checkJs": true`                        |
| **`sourceMap`**           | Generates `.map` files for debugging in browsers.                                                | `"sourceMap": true`                      |
| **`esModuleInterop`**     | Ensures compatibility with ES6-style module imports in CommonJS modules.                         | `"esModuleInterop": true`                |
| **`declaration`**         | Generates `.d.ts` files for external use.                                                        | `"declaration": true`                    |

---

### **Step 5: Working Example**

**Project Structure**
```plaintext
typescript-example/
├── src/
│   └── index.ts
├── dist/
├── tsconfig.json
└── package.json
```

#### **Source File: `src/index.ts`**
```typescript
const greet = (name: string): string => {
    return `Hello, ${name}!`;
};
console.log(greet("TypeScript"));
```

#### **Configuration: `tsconfig.json`**
```json
{
  "compilerOptions": {
    "target": "ES6",
    "module": "commonjs",
    "strict": true,
    "outDir": "./dist",
    "rootDir": "./src",
    "esModuleInterop": true
  },
  "include": ["src/**/*"]
}
```

#### **Compile the Code**
Run the TypeScript compiler:
```bash
tsc
```

**Output Directory**
```plaintext
typescript-example/
├── dist/
│   └── index.js
└── src/
    └── index.ts
```

#### **Compiled JavaScript: `dist/index.js`**
```javascript
"use strict";
const greet = (name) => {
    return `Hello, ${name}!`;
};
console.log(greet("TypeScript"));
```

Run the output using Node.js:
```bash
node dist/index.js
```

---

### **Common Errors and Fixes**
1. **Error: Cannot find module or type declaration**
   - Ensure `esModuleInterop` is enabled for compatibility with default exports.
   - Install type declarations for third-party libraries using `@types`.

2. **Error: `src` and `dist` directories not defined**
   - Use `rootDir` and `outDir` to organize source and output directories.

3. **File Inclusion Issues**
   - Adjust `include` and `exclude` options to specify the files/folders.

---

### **Interview Questions with Answers**

#### **1. What is the role of `tsconfig.json` in TypeScript?**
**Answer**:  
`tsconfig.json` is the configuration file for TypeScript projects. It defines compiler options, directories to include or exclude, and rules for type checking. This central file helps manage project-specific settings.

---

#### **2. Explain the difference between `rootDir` and `outDir` in TypeScript configuration.**
**Answer**:  
- `rootDir`: Specifies the directory containing the source TypeScript files.  
- `outDir`: Specifies the directory where the compiled JavaScript files will be placed.

Example:
```json
{
  "rootDir": "./src",
  "outDir": "./dist"
}
```

---

#### **3. How does the `strict` mode improve TypeScript development?**
**Answer**:  
`strict` mode enables a set of strict type-checking rules, such as:
- **`noImplicitAny`**: Prevents variables from implicitly being of type `any`.
- **`strictNullChecks`**: Ensures null and undefined are handled explicitly.
- **`strictBindCallApply`**: Improves type safety for `bind`, `call`, and `apply`.

This ensures better type safety and reduces runtime errors.

---

#### **4. What is the use of the `esModuleInterop` option?**
**Answer**:  
`esModuleInterop` allows TypeScript to handle ES6-style default imports in CommonJS modules. Without it, developers would need to use `import * as` syntax for CommonJS modules.

Example:
```typescript
import lodash from 'lodash'; // Works with esModuleInterop
```

---

#### **5. How do you enable debugging for TypeScript projects?**
**Answer**:  
To enable debugging, use the `sourceMap` option in `tsconfig.json`. This generates `.map` files that allow you to debug TypeScript code in a browser or IDE.

```json
{
  "compilerOptions": {
    "sourceMap": true
  }
}
```

---

## **Complete Guide to TypeScript Types: Assertions**

TypeScript **type assertions** allow you to override the inferred or declared type of a value. Assertions provide a way to tell the compiler more about the type of a value in situations where TypeScript's type inference may fall short. 

This guide will explain:
1. **What are Type Assertions?**
2. **`as [type]` Assertion**
3. **`as any` Assertion**
4. **`as const` Assertion**

Each section includes examples and interview questions with answers.

---

## **1. What are Type Assertions?**

Type assertions let you inform TypeScript about the specific type of a value when:
1. TypeScript cannot automatically infer the type.
2. You know more about the value's type than the compiler.

TypeScript uses the `as` keyword for assertions:
```typescript
const value: unknown = "TypeScript";
const strLength: number = (value as string).length;
```

---

## **2. `as [type]` Assertion**

This is the most common type assertion. It tells TypeScript to treat a value as a specific type. 

### **Syntax**
```typescript
value as TargetType
```

### **When to Use**
- **Casting unknown values**: You know the value type but TypeScript doesn’t.
- **Working with DOM elements**: TypeScript cannot infer the exact element type.
- **Overriding inferred types**: To refine or narrow down the type.

### **Example 1: Casting `unknown` to a Known Type**
```typescript
const value: unknown = "Hello, TypeScript!";
const length: number = (value as string).length; // Tell TypeScript it’s a string
console.log(length); // Outputs: 17
```

### **Example 2: Working with DOM Elements**
```typescript
const inputElement = document.getElementById("username") as HTMLInputElement;
inputElement.value = "TypeScript Rocks!";
```

### **Example 3: Narrowing a Union Type**
```typescript
type Shape = { kind: "circle"; radius: number } | { kind: "square"; side: number };

const shape: Shape = { kind: "circle", radius: 10 };

if (shape.kind === "circle") {
    console.log((shape as { kind: "circle"; radius: number }).radius); // Safe assertion
}
```

---

### **Interview Questions**
1. **What is a type assertion in TypeScript?**  
   **Answer**: A type assertion is a way to explicitly tell the TypeScript compiler to treat a value as a specific type when TypeScript cannot infer it or infers it incorrectly.

2. **When should you use `as [type]`?**  
   **Answer**: Use `as [type]` when you are certain of the value's type and need to override TypeScript's inferred type, such as working with `unknown` values or narrowing union types.

---

## **3. `as any` Assertion**

The `any` type disables type checking, allowing you to assign any value to any type. The `as any` assertion tells TypeScript to treat the value as `any`, bypassing type-checking completely.

### **Syntax**
```typescript
value as any
```

### **When to Use**
- Avoid using `as any` unless absolutely necessary.
- Suitable for gradual migration from JavaScript to TypeScript when type information is not yet available.

### **Example 1: Suppressing Type Errors**
```typescript
const value: unknown = "TypeScript!";
const unsafe: number = (value as any).length; // TypeScript doesn’t check the type
console.log(unsafe); // Outputs: 10
```

### **Example 2: Working with Third-Party Libraries**
Sometimes libraries lack proper type definitions, and you might use `as any` temporarily:
```typescript
declare const untypedLibrary: any;

const result = untypedLibrary.someFunction() as any; // Suppress type errors
console.log(result);
```

---

### **Why Avoid `as any`?**
- It undermines TypeScript’s type safety, introducing potential runtime errors.
- Use only as a last resort or in legacy codebases.

---

### **Interview Questions**
1. **What is the `any` type in TypeScript, and how does `as any` work?**  
   **Answer**: The `any` type disables type checking, allowing any operation on the value. `as any` is a type assertion that explicitly tells TypeScript to treat the value as `any`.

2. **What are the risks of using `as any` in TypeScript?**  
   **Answer**: It disables type safety, negating the benefits of TypeScript, and may introduce runtime errors due to unchecked operations.

---

## **4. `as const` Assertion**

The `as const` assertion tells TypeScript to infer a literal type rather than a more general type. It converts an object or array into a **readonly** form where all values are treated as constants.

### **Syntax**
```typescript
value as const
```

### **When to Use**
- Use `as const` when you need immutable values.
- Helpful for defining fixed configurations, enums, or literal types.

### **Example 1: Freezing an Object**
```typescript
const config = {
    apiUrl: "https://api.example.com",
    timeout: 5000
} as const;

// TypeScript infers:
// {
//   readonly apiUrl: "https://api.example.com";
//   readonly timeout: 5000;
// }
```
Attempting to modify `config.apiUrl` will throw an error:
```typescript
// config.apiUrl = "https://new-url.com"; // Error: Cannot assign to 'apiUrl' because it is a read-only property.
```

### **Example 2: Arrays with Literal Types**
```typescript
const colors = ["red", "green", "blue"] as const;

// TypeScript infers: readonly ["red", "green", "blue"]
// colors.push("yellow"); // Error: Property 'push' does not exist on type 'readonly ["red", "green", "blue"]'
```

### **Example 3: Enforcing Literal Types**
```typescript
type HTTPMethod = "GET" | "POST" | "DELETE";
const method = "GET" as const;

// The type of `method` is now `"GET"` instead of `string`.
const request = (url: string, method: HTTPMethod) => {
    console.log(`Requesting ${url} with ${method}`);
};

request("/api/resource", method); // Valid
```

---

### **Benefits of `as const`**
- Enforces immutability and literal types.
- Prevents accidental changes to values.
- Reduces bugs in configurations or enums.

---

### **Interview Questions**
1. **What does `as const` do in TypeScript?**  
   **Answer**: The `as const` assertion converts an object or array into an immutable (readonly) form and infers literal types for its values.

2. **When should you use `as const`?**  
   **Answer**: Use `as const` when you want to freeze a value and ensure its type is inferred as literal and readonly, such as for configurations, enums, or constant arrays.

3. **How does `as const` improve TypeScript type inference?**  
   **Answer**: It ensures values are inferred with literal types, reducing ambiguity and improving type safety in cases like string literals or numeric constants.

---

### **Key Takeaways**
- **`as [type]`**: Use when you need to cast or narrow a type to something more specific.
- **`as any`**: A last-resort option to suppress type checking, should be used sparingly.
- **`as const`**: Converts a value into a literal and readonly type, perfect for immutability.

# **Complete Guide to TypeScript: Non-Null Assertions and the `satisfies` Keyword**

TypeScript provides features like **non-null assertions** and the **`satisfies`** keyword to handle specific cases where its type inference and checking system might fall short. These tools allow developers to write safer and more expressive code when dealing with types.

---

## **1. Non-Null Assertions**

In TypeScript, values can potentially be `null` or `undefined`, depending on the context. The **non-null assertion operator (`!`)** allows you to explicitly tell TypeScript that a value is neither `null` nor `undefined`, overriding its default checks.

### **When to Use Non-Null Assertions**
- Use when you are absolutely certain that a value is not `null` or `undefined` but TypeScript cannot infer it.
- Commonly used in scenarios involving:
  - DOM manipulation.
  - Narrowing types in conditional code.
  - Optional chaining fallbacks.

---

### **Syntax**
```typescript
value!;
```

---

### **Example 1: DOM Manipulation**
```typescript
const button = document.getElementById("submit-button")!;
button.addEventListener("click", () => {
    console.log("Button clicked!");
});
```

**Explanation**:
- `document.getElementById` may return `null` if the element does not exist.
- The `!` operator tells TypeScript to ignore this possibility.

---

### **Example 2: Narrowing in Conditional Statements**
```typescript
function getLength(str?: string): number {
    return str!.length; // Using `!` because we know `str` is not null/undefined
}

console.log(getLength("TypeScript")); // Outputs: 10
```

---

### **Example 3: Chained Optional Access**
```typescript
type User = {
    name?: {
        first: string;
        last: string;
    };
};

const user: User = { name: { first: "John", last: "Doe" } };
console.log(user.name!.first); // Using `!` to assert `name` is not undefined
```

---

### **When NOT to Use Non-Null Assertions**
- Avoid overusing it, as incorrect assertions can cause runtime errors.
- Prefer proper null checks or using `strictNullChecks`.

---

### **Interview Questions**
1. **What is the purpose of the non-null assertion operator (`!`) in TypeScript?**  
   **Answer**: It tells TypeScript to treat a value as non-null and non-undefined, overriding its default type checks.

2. **What are the risks of using the non-null assertion operator?**  
   **Answer**: If the value is actually `null` or `undefined` at runtime, it will result in an error. Misuse of this operator can lead to unstable code.

3. **When should you avoid using non-null assertions?**  
   **Answer**: Avoid using it when proper null checking or optional chaining (`?.`) can handle the case more safely.

---

---

## **2. The `satisfies` Keyword**

The `satisfies` keyword, introduced in TypeScript 4.9, is used to ensure that a value conforms to a specific type without changing the inferred type of that value. It helps enforce more precise constraints while retaining TypeScript's type inference.

### **When to Use the `satisfies` Keyword**
- Use when you need to:
  - Validate that an object meets the requirements of an interface/type.
  - Ensure stricter type checking while maintaining inference for properties.
  - Provide detailed constraints in complex data structures.

---

### **Syntax**
```typescript
const value = someExpression satisfies SomeType;
```

---

### **Example 1: Enforcing Object Structure**
```typescript
type Config = {
    apiUrl: string;
    timeout: number;
};

const config = {
    apiUrl: "https://api.example.com",
    timeout: 5000,
    extra: "this is extra"
} satisfies Config;

// Valid because `config` satisfies `Config`, but retains the `extra` property in inference
```

**Explanation**:
- The object is validated against `Config`.
- The `extra` property is ignored by `satisfies` but retained in `config`.

---

### **Example 2: Validating String Literals**
```typescript
type HTTPMethod = "GET" | "POST" | "PUT" | "DELETE";

const method = "GET" satisfies HTTPMethod;
// Valid, as "GET" matches the `HTTPMethod` type
```

**Explanation**:
- `method` is inferred as `"GET"`, and TypeScript ensures it adheres to `HTTPMethod`.

---

### **Example 3: Arrays with Literals**
```typescript
type Colors = "red" | "green" | "blue";

const colorArray = ["red", "green"] satisfies Colors[];

// TypeScript ensures all elements of `colorArray` match the `Colors` type
```

---

### **Example 4: Preventing Looser Type Inference**
```typescript
type ExactConfig = {
    apiUrl: string;
    timeout: number;
};

const badConfig = {
    apiUrl: "https://api.example.com",
    timeout: "5000" // Error: timeout must be a number
} satisfies ExactConfig;
```

**Explanation**:
- The `timeout` property in `badConfig` does not match the `ExactConfig` type, and TypeScript raises an error.

---

### **Benefits of the `satisfies` Keyword**
1. **Type Validation Without Losing Inference**: Ensures that objects meet the requirements of a type while keeping additional properties.
2. **Improves Readability and Safety**: Makes code self-documenting and ensures strict type constraints.

---

### **Interview Questions**
1. **What is the purpose of the `satisfies` keyword in TypeScript?**  
   **Answer**: The `satisfies` keyword validates that a value adheres to a specific type while retaining the value's inferred type.

2. **How is `satisfies` different from `as` assertions?**  
   **Answer**: The `as` keyword overrides TypeScript’s type inference, potentially introducing runtime errors if misused. The `satisfies` keyword ensures a value meets type requirements without altering its inferred type.

3. **When should you prefer `satisfies` over explicit type annotations?**  
   **Answer**: Use `satisfies` when you want to enforce a type constraint without discarding extra properties or altering the inferred type.

---

---

## **Comparison of Non-Null Assertions and `satisfies`**

| **Feature**            | **Non-Null Assertion**                        | **`satisfies` Keyword**                          |
|-------------------------|-----------------------------------------------|-------------------------------------------------|
| **Purpose**             | Overrides TypeScript’s null/undefined checks | Validates a value against a specific type       |
| **Syntax**              | `value!`                                     | `value satisfies Type`                          |
| **Effect on Type**      | Narrows type to non-null                     | Retains the original inferred type             |
| **Use Cases**           | Working with DOM or nullish values           | Enforcing constraints without losing inference |

---

### **Key Takeaways**
1. **Non-Null Assertions (`!`)**:
   - Use sparingly to tell TypeScript a value is not null/undefined.
   - Risky if misused, as runtime errors can occur.

2. **`satisfies` Keyword**:
   - Ensures a value conforms to a type without changing its inferred type.
   - Use to enforce constraints while retaining additional properties or details.

# **Complete Guide to TypeScript Primitive Types**

In TypeScript, **primitive types** are the basic building blocks of the type system. These include `number`, `boolean`, `string`, `undefined`, `void`, and `null`. Understanding these types is crucial for building type-safe applications.

---

## **1. `number`**

The `number` type in TypeScript represents all numeric values, including integers and floating-point numbers.

### **Examples**
```typescript
let age: number = 30;          // Integer
let price: number = 99.99;     // Floating-point number
let hex: number = 0xff;        // Hexadecimal
let binary: number = 0b1010;   // Binary
let octal: number = 0o744;     // Octal
```

### **Operations**
```typescript
const a: number = 10;
const b: number = 20;
const sum: number = a + b; // Addition
const product: number = a * b; // Multiplication
console.log(sum, product); // Outputs: 30, 200
```

### **Common Use Cases**
- Mathematical calculations.
- Representing numerical properties like `age`, `price`, `height`, etc.

---

### **Interview Questions**
1. **What types of numbers does the `number` type in TypeScript support?**  
   **Answer**: The `number` type supports integers, floating-point numbers, hexadecimal, binary, and octal representations.

2. **What will happen if you assign a string to a variable of type `number`?**  
   **Answer**: TypeScript will throw a compilation error.

---

## **2. `boolean`**

The `boolean` type represents logical values: `true` or `false`.

### **Examples**
```typescript
let isCompleted: boolean = true;
let hasErrors: boolean = false;
```

### **Operations**
```typescript
const a: boolean = true;
const b: boolean = false;

console.log(a && b); // Logical AND: false
console.log(a || b); // Logical OR: true
console.log(!a);     // Logical NOT: false
```

### **Common Use Cases**
- Conditional checks.
- Flags to represent states like `isLoggedIn`, `isAdmin`, etc.

---

### **Interview Questions**
1. **Can you assign `0` or `1` to a `boolean` variable in TypeScript?**  
   **Answer**: No, TypeScript only allows `true` or `false` as boolean values.

2. **What is the default value of a boolean variable in TypeScript?**  
   **Answer**: A boolean variable is undefined unless explicitly assigned a value.

---

## **3. `string`**

The `string` type represents textual data.

### **Examples**
```typescript
let greeting: string = "Hello, TypeScript!";
let name: string = 'John';
let template: string = `Welcome, ${name}!`; // Template literals
```

### **Operations**
```typescript
const str1: string = "Hello";
const str2: string = "World";

console.log(str1 + " " + str2); // String concatenation: "Hello World"
console.log(`Length: ${str1.length}`); // Access properties: "Length: 5"
```

### **Common Use Cases**
- Representing textual information like names, messages, and content.
- Interpolating values using template literals.

---

### **Interview Questions**
1. **How do template literals differ from regular string literals?**  
   **Answer**: Template literals allow embedded expressions and multi-line strings using backticks (\`), while regular string literals use quotes (`'` or `"`).

2. **What methods are available for `string` in TypeScript?**  
   **Answer**: Some common methods include `toUpperCase()`, `toLowerCase()`, `substring()`, `split()`, and `replace()`.

---

## **4. `undefined`**

The `undefined` type is used when a variable is declared but not assigned a value.

### **Examples**
```typescript
let uninitialized: undefined;
console.log(uninitialized); // Outputs: undefined
```

### **Default Value**
Variables that are declared but not assigned a value default to `undefined`.

### **Common Use Cases**
- Checking whether a variable has been initialized.
- Optional parameters in functions.

```typescript
function greet(message?: string): void {
    console.log(message); // If not passed, message is undefined
}
```

---

### **Interview Questions**
1. **How does TypeScript handle variables of type `undefined`?**  
   **Answer**: A variable can explicitly be declared as `undefined` or left uninitialized.

2. **What is the difference between `undefined` and `null` in TypeScript?**  
   **Answer**: `undefined` means a variable has been declared but not initialized, while `null` explicitly represents an intentional absence of value.

---

## **5. `void`**

The `void` type is used to represent the absence of any value. It is typically used as the return type for functions that do not return anything.

### **Examples**
```typescript
function logMessage(message: string): void {
    console.log(message);
}
```

### **Assigning a `void` Value**
You cannot assign `void` values to variables, except when using `undefined` in certain cases.

### **Common Use Cases**
- Functions or methods that only perform side effects without returning a value.

---

### **Interview Questions**
1. **What is the purpose of the `void` type in TypeScript?**  
   **Answer**: It is used to indicate that a function does not return any value.

2. **Can you assign a value to a `void` variable?**  
   **Answer**: No, except `undefined` in certain contexts.

---

## **6. `null`**

The `null` type represents the intentional absence of any object value.

### **Examples**
```typescript
let empty: null = null;
```

### **Default Behavior**
- By default, `null` is assignable to variables of type `any`, `unknown`, or their explicit union types (e.g., `string | null`).

---

### **Common Use Cases**
- Representing the absence of a value in APIs or optional fields.

```typescript
type User = {
    name: string;
    age: number | null; // Age is optional
};

const user: User = {
    name: "Alice",
    age: null, // Indicates no age provided
};
```

---

### **Interview Questions**
1. **What is the difference between `undefined` and `null`?**  
   **Answer**: `undefined` means a variable has been declared but not initialized, while `null` explicitly represents no value.

2. **How does `strictNullChecks` affect the usage of `null`?**  
   **Answer**: When `strictNullChecks` is enabled, `null` is not assignable to other types unless explicitly included in their type definition.

---

---

## **Comparison of Primitive Types**

| **Type**     | **Description**                             | **Example**                                      |
|--------------|---------------------------------------------|-------------------------------------------------|
| `number`     | Represents numeric values.                  | `let age: number = 25;`                        |
| `boolean`    | Represents logical `true` or `false`.       | `let isActive: boolean = true;`                |
| `string`     | Represents textual data.                    | `let name: string = "TypeScript";`             |
| `undefined`  | Represents an uninitialized variable.       | `let x: undefined;`                            |
| `void`       | Represents functions that don’t return a value. | `function log(): void { console.log("Log"); }` |
| `null`       | Represents the intentional absence of value.| `let value: null = null;`                      |

---

### **Key Takeaways**
- Primitive types are fundamental in TypeScript for building type-safe applications.
- Use `null` and `undefined` cautiously, especially with `strictNullChecks`.
- Leverage `void` for functions that perform side effects but do not return values.

# **Complete Guide to TypeScript Object Types**

TypeScript provides powerful tools for defining object-like structures through its object types, enabling developers to model real-world data with precision. The key object types include **interfaces**, **classes**, **enums**, **arrays**, **tuples**, and generic **objects**.

---

## **1. Interface**

An **interface** in TypeScript defines the shape of an object. It specifies the structure, including the properties and their types.

### **Defining Interfaces**
```typescript
interface Person {
    name: string;
    age: number;
    isEmployed?: boolean; // Optional property
}
```

### **Examples**
#### **Basic Example**
```typescript
const employee: Person = {
    name: "Alice",
    age: 30,
    isEmployed: true,
};
```

#### **Using Readonly Properties**
```typescript
interface Car {
    readonly make: string; // Cannot be modified
    model: string;
}

const myCar: Car = { make: "Toyota", model: "Corolla" };
// myCar.make = "Honda"; // Error: Cannot assign to 'make'
```

#### **Extending Interfaces**
```typescript
interface Animal {
    species: string;
}

interface Dog extends Animal {
    breed: string;
}

const myDog: Dog = {
    species: "Canine",
    breed: "Labrador",
};
```

---

### **Interview Questions**
1. **What is the difference between an interface and a type alias?**  
   **Answer**: While both can define object shapes, interfaces support declaration merging and are often preferred for defining object shapes.

2. **Can interfaces have optional or readonly properties?**  
   **Answer**: Yes, use `?` for optional properties and `readonly` for immutable properties.

---

## **2. Class**

A **class** in TypeScript is a blueprint for creating objects. It can include properties, methods, and access modifiers like `public`, `private`, and `protected`.

### **Defining a Class**
```typescript
class Animal {
    name: string;

    constructor(name: string) {
        this.name = name;
    }

    makeSound(): void {
        console.log(`${this.name} is making a sound.`);
    }
}
```

### **Inheritance**
```typescript
class Dog extends Animal {
    constructor(name: string) {
        super(name);
    }

    makeSound(): void {
        console.log(`${this.name} is barking.`);
    }
}

const myDog = new Dog("Buddy");
myDog.makeSound(); // Buddy is barking.
```

### **Access Modifiers**
- **`public`**: Accessible anywhere.
- **`private`**: Accessible only within the class.
- **`protected`**: Accessible within the class and subclasses.

```typescript
class BankAccount {
    private balance: number = 1000;

    deposit(amount: number): void {
        this.balance += amount;
    }

    getBalance(): number {
        return this.balance;
    }
}

const account = new BankAccount();
account.deposit(500);
// console.log(account.balance); // Error: 'balance' is private
console.log(account.getBalance()); // Outputs: 1500
```

---

### **Interview Questions**
1. **What are the differences between `public`, `private`, and `protected`?**  
   **Answer**: `public` is accessible everywhere, `private` is restricted to the class, and `protected` is accessible within the class and its subclasses.

2. **What is the role of the `super` keyword in TypeScript?**  
   **Answer**: `super` is used to call the constructor or methods of a parent class in a subclass.

---

## **3. Enum**

Enums allow you to define a set of named constants, improving code readability and type safety.

### **Defining Enums**
```typescript
enum Direction {
    North,
    South,
    East,
    West,
}
```

### **Examples**
#### **Basic Enum Usage**
```typescript
const move = (dir: Direction): void => {
    console.log(`Moving ${Direction[dir]}`);
};

move(Direction.North); // Outputs: Moving North
```

#### **String Enums**
```typescript
enum Colors {
    Red = "RED",
    Green = "GREEN",
    Blue = "BLUE",
}

const favoriteColor: Colors = Colors.Blue;
console.log(favoriteColor); // Outputs: BLUE
```

---

### **Interview Questions**
1. **What are the advantages of using enums in TypeScript?**  
   **Answer**: Enums improve code readability, prevent magic numbers, and provide type-safe named constants.

2. **What is the difference between numeric and string enums?**  
   **Answer**: Numeric enums map values to numbers (default or custom), while string enums map values to specific strings.

---

## **4. Array**

An **array** in TypeScript is a collection of values of the same type.

### **Defining Arrays**
```typescript
let numbers: number[] = [1, 2, 3, 4];
let strings: Array<string> = ["a", "b", "c"];
```

### **Examples**
#### **Basic Array Operations**
```typescript
const fruits: string[] = ["apple", "banana"];
fruits.push("cherry");
console.log(fruits); // Outputs: ['apple', 'banana', 'cherry']
```

#### **Using Readonly Arrays**
```typescript
const readonlyFruits: readonly string[] = ["apple", "banana"];
// readonlyFruits.push("cherry"); // Error: Cannot push to readonly array
```

---

### **Interview Questions**
1. **How do you define a readonly array in TypeScript?**  
   **Answer**: Use `readonly` before the array type (`readonly string[]`).

2. **Can an array in TypeScript contain multiple types?**  
   **Answer**: Yes, use a union type or `any[]`.

---

## **5. Tuple**

A **tuple** is a fixed-size array with specific types for each element.

### **Defining Tuples**
```typescript
let tuple: [string, number];
tuple = ["age", 25]; // Valid
// tuple = [25, "age"]; // Error: Type 'number' is not assignable to type 'string'
```

### **Examples**
#### **Using Tuples**
```typescript
const person: [string, number] = ["Alice", 30];
const [name, age] = person;
console.log(name, age); // Outputs: Alice, 30
```

#### **Optional Tuple Elements**
```typescript
let optionalTuple: [string, number?];
optionalTuple = ["Alice"];
```

---

### **Interview Questions**
1. **What is the difference between an array and a tuple?**  
   **Answer**: Arrays can contain elements of the same type with no fixed length, while tuples have a fixed length with specific types for each element.

2. **How do you access elements of a tuple in TypeScript?**  
   **Answer**: Access tuple elements using array indexing (`tuple[0]`).

---

## **6. Object**

The `object` type represents non-primitive values such as arrays, functions, or objects.

### **Defining Objects**
```typescript
let user: { name: string; age: number } = { name: "John", age: 25 };
```

### **Examples**
#### **Object with Optional Properties**
```typescript
let book: { title: string; author?: string } = { title: "TypeScript Handbook" };
```

#### **Index Signatures**
```typescript
interface Dictionary {
    [key: string]: string;
}

const dict: Dictionary = { hello: "world", welcome: "home" };
```

---

### **Interview Questions**
1. **What is the `object` type in TypeScript?**  
   **Answer**: It represents non-primitive values like arrays, functions, or custom objects.

2. **How do you define an object with optional properties?**  
   **Answer**: Use the `?` syntax for optional properties.

---

---

### **Comparison Table**

| **Type**     | **Description**                                                                                      | **Example**                                |
|--------------|------------------------------------------------------------------------------------------------------|--------------------------------------------|
| Interface    | Defines the structure of an object.                                                                 | `interface Person { name: string; }`      |
| Class        | A blueprint for creating objects with properties and methods.                                        | `class Animal { name: string; }`          |
| Enum         | Defines a set of named constants.                                                                    | `enum Colors { Red, Green }`              |
| Array        | Represents a collection of elements of the same type.                                                | `let arr: number[] = [1, 2, 3];`          |
| Tuple        | Represents a fixed-size array with specific types for each element.                                  | `let tuple: [string, number] = ["a", 1];` |
| Object       | Represents a non-primitive value.                                                                    | `let obj: { name: string; } = { name: ""}`|


# **Complete Guide to TypeScript: Top Types and Bottom Types**

In TypeScript, **top types** and **bottom types** represent the bounds of the type hierarchy. These types play a critical role in type safety and expressiveness within TypeScript's type system.

---

## **1. Top Types**

### **Definition**  
- **Top types** are the "widest" types in TypeScript's hierarchy.
- They can hold values of any type.
- The two primary top types are:
  - **`unknown`**: The type-safe top type.
  - **`any`**: The unsound top type that disables type checking.

---

### **1a. `unknown`**

The `unknown` type represents any value, but unlike `any`, you cannot directly operate on it without narrowing its type.

#### **Key Features**
- Safer than `any`.
- Requires type checks before usage.
- Suitable for values whose types are not known at compile time.

#### **Examples**

##### **Example 1: Declaring `unknown`**
```typescript
let value: unknown;

value = "Hello"; // Valid
value = 42;      // Valid
value = true;    // Valid
```

##### **Example 2: Type Narrowing**
```typescript
function handleInput(input: unknown): void {
    if (typeof input === "string") {
        console.log(input.toUpperCase()); // Allowed after narrowing
    } else {
        console.log("Input is not a string.");
    }
}

handleInput("Hello"); // Outputs: HELLO
handleInput(42);      // Outputs: Input is not a string.
```

##### **Example 3: Type Guards with `instanceof`**
```typescript
function processValue(value: unknown): void {
    if (value instanceof Date) {
        console.log(value.toISOString()); // Allowed after narrowing
    } else {
        console.log("Not a date.");
    }
}

processValue(new Date()); // Outputs the ISO string
processValue("Hello");    // Outputs: Not a date.
```

---

#### **Interview Questions**
1. **What is the difference between `unknown` and `any` in TypeScript?**  
   **Answer**: Both can hold values of any type, but `unknown` requires explicit type checks before operations, making it safer than `any`.

2. **When should you use `unknown` over `any`?**  
   **Answer**: Use `unknown` when you want to enforce type safety and require explicit checks before using the value.

---

### **1b. `any`**

The `any` type represents a value that can bypass all type checks. It disables TypeScript's type safety, making it the least restrictive type.

#### **Key Features**
- Assignable to and from any type.
- Operations on `any` bypass compile-time checks.
- Useful for gradual migration or working with untyped third-party libraries.

#### **Examples**

##### **Example 1: Declaring `any`**
```typescript
let value: any;

value = "Hello"; // Valid
value = 42;      // Valid
value = {};      // Valid
```

##### **Example 2: No Type Checking**
```typescript
let input: any = "Hello";

console.log(input.toUpperCase()); // No type error, but risky if `input` changes
input = 42;
console.log(input.toUpperCase()); // Runtime error: toUpperCase is not a function
```

##### **Example 3: Mixing Types**
```typescript
let mixed: any = [1, "two", true];
console.log(mixed[1]); // Outputs: two
```

---

#### **Risks of `any`**
- Removes type safety.
- Increases the likelihood of runtime errors.

---

#### **Interview Questions**
1. **What is the primary disadvantage of using `any` in TypeScript?**  
   **Answer**: It disables type checking, leading to potential runtime errors and negating the benefits of TypeScript's type system.

2. **When should you use `any` in TypeScript?**  
   **Answer**: Use it sparingly, typically when dealing with untyped libraries or during the gradual migration of JavaScript codebases.

---

---

## **2. Bottom Types**

### **Definition**  
- **Bottom types** are the "narrowest" types in TypeScript's hierarchy.
- They represent values that can never occur.
- The primary bottom type is **`never`**.

---

### **2a. `never`**

The `never` type represents values that never occur. It is used to indicate unreachable code or impossible states.

#### **Key Features**
- A function that never returns (e.g., throws an error or infinite loops) returns `never`.
- Can be used for exhaustive type checking in switch cases.
- Cannot be assigned any value.

#### **Examples**

##### **Example 1: Functions That Throw Errors**
```typescript
function throwError(message: string): never {
    throw new Error(message);
}
```

##### **Example 2: Infinite Loops**
```typescript
function infiniteLoop(): never {
    while (true) {
        console.log("Looping forever...");
    }
}
```

##### **Example 3: Exhaustive Checks**
```typescript
type Shape = { kind: "circle"; radius: number } | { kind: "square"; side: number };

function getArea(shape: Shape): number {
    switch (shape.kind) {
        case "circle":
            return Math.PI * shape.radius ** 2;
        case "square":
            return shape.side ** 2;
        default:
            // Ensures all cases are handled
            const _exhaustiveCheck: never = shape;
            throw new Error("Unhandled case: " + _exhaustiveCheck);
    }
}
```

---

#### **When to Use `never`**
- Define functions that never return.
- Use in unreachable code paths.
- Enforce exhaustive checks in discriminated unions.

---

#### **Interview Questions**
1. **What is the `never` type in TypeScript?**  
   **Answer**: It represents a type that contains no values, used for functions that never return or code paths that cannot be executed.

2. **How is `never` different from `void`?**  
   **Answer**: `void` represents functions that return no value, while `never` represents functions that do not return at all (e.g., throw errors or infinite loops).

3. **Why is `never` useful for exhaustive type checks?**  
   **Answer**: It ensures that all possible cases in a discriminated union are handled, improving type safety.

---

---

## **Comparison of Top and Bottom Types**

| **Type**   | **Category** | **Description**                                                                 |
|------------|--------------|---------------------------------------------------------------------------------|
| `unknown`  | Top Type     | Represents any value but requires type checks before operations.                |
| `any`      | Top Type     | Represents any value without type checking, disabling type safety.              |
| `never`    | Bottom Type  | Represents values that never occur, used for unreachable code or exhaustive checks. |

---

### **Key Takeaways**
1. Use **`unknown`** for safe handling of values with unknown types.
2. Use **`any`** sparingly to avoid losing type safety.
3. Use **`never`** to model unreachable code and ensure exhaustive type checking.

# **Complete Guide to TypeScript: Type Inference**

TypeScript uses **type inference** to automatically determine the type of a variable or expression without requiring explicit type annotations. This feature helps reduce verbosity while maintaining type safety.

---

## **What is Type Inference?**

Type inference is the ability of TypeScript to deduce the type of a variable, parameter, or return value based on the context. TypeScript infers types when:
1. A variable is initialized.
2. A default value is assigned to a function parameter.
3. A function's return type is derived from its return statement.

---

## **Examples of Type Inference**

### **1. Variable Initialization**
When you assign a value to a variable, TypeScript infers its type.

```typescript
let count = 42; // TypeScript infers `count` as `number`
// count = "forty-two"; // Error: Type 'string' is not assignable to type 'number'

const greeting = "Hello, TypeScript!"; // Inferred as `string`
```

---

### **2. Function Return Types**
The return type of a function is inferred from its return statements.

```typescript
function add(a: number, b: number) {
    return a + b; // TypeScript infers return type as `number`
}

const sum = add(5, 10); // `sum` is inferred as `number`
```

---

### **3. Array Inference**
The type of an array is inferred based on its elements.

```typescript
const numbers = [1, 2, 3]; // Inferred as `number[]`
const mixed = [1, "two", true]; // Inferred as `(number | string | boolean)[]`
```

---

### **4. Default Parameters**
When default values are provided for function parameters, TypeScript infers their types.

```typescript
function greet(message = "Hello"): string {
    return message.toUpperCase(); // Return type inferred as `string`
}
```

---

### **5. Contextual Typing**
Type inference works based on the context in which a value is used, such as in event handlers or callbacks.

```typescript
const button = document.querySelector("button");
button?.addEventListener("click", (event) => {
    console.log(event.target); // `event` is inferred as `MouseEvent`
});
```

---

### **6. Type Assertions**
When you assert a type, you override the inference system to specify a more specific type.

```typescript
const value = "42" as unknown; // Inferred as `unknown`
const numericValue = value as string; // Asserted as `string`
```

---

## **Advanced Type Inference Features**

### **1. Best Common Type**
When inferring the type of an array or union, TypeScript determines the "best common type."

```typescript
const values = [1, "hello", true]; // Inferred as `(number | string | boolean)[]`
```

### **2. Return Type Inference in Callbacks**
TypeScript infers the return type of a callback function based on its context.

```typescript
const numbers = [1, 2, 3];
const doubled = numbers.map((num) => num * 2); // Inferred as `number[]`
```

---

## **When Explicit Types Are Needed**

### **1. Ambiguity in Inference**
Explicit types are required when TypeScript cannot determine the type accurately.

```typescript
let value; // Inferred as `any`
value = 42;
value = "hello";
```

Solution:
```typescript
let value: string;
value = "hello"; // Type-safe assignment
```

---

### **2. Function Return Types in Complex Scenarios**
Explicit return types are necessary when the return type is ambiguous or needs to be more specific.

```typescript
function fetchData(): Promise<string> {
    return new Promise((resolve) => resolve("Data loaded")); // Explicit return type
}
```

---

## **Interview Questions**

### **Basic Questions**
1. **What is type inference in TypeScript?**  
   **Answer**: Type inference is TypeScript’s ability to automatically deduce the type of a variable, parameter, or return value based on its usage or initialization.

2. **What is contextual typing in TypeScript?**  
   **Answer**: Contextual typing is when TypeScript infers types based on the context, such as event listeners or callbacks.

---

### **Advanced Questions**
1. **How does TypeScript infer the type of an array with mixed elements?**  
   **Answer**: TypeScript infers the type as a union of all possible element types in the array.

2. **When should you use explicit types instead of relying on type inference?**  
   **Answer**: Use explicit types when inference results in ambiguity, such as uninitialized variables or complex return types.

3. **What is the "best common type" in TypeScript?**  
   **Answer**: The best common type is the shared type among elements in an array or union, used by TypeScript for inference.

---

## **Key Takeaways**
1. Type inference reduces verbosity while maintaining type safety.
2. TypeScript infers types based on initial values, return statements, or usage contexts.
3. Use explicit types in cases of ambiguity or for more precise control.

# **Complete Guide to TypeScript: Type Compatibility**

TypeScript's **type compatibility** system determines whether a type can be assigned to another type. It is a core feature of TypeScript's structural type system, which ensures flexibility and type safety.

---

## **What is Type Compatibility?**

Type compatibility in TypeScript is based on **structural typing**, meaning compatibility is determined by comparing the structure (properties and methods) of two types rather than their explicit declarations.  

For example:
```typescript
interface Point {
    x: number;
    y: number;
}

const point: Point = { x: 10, y: 20 }; // Compatible
```

---

## **1. Basic Type Compatibility**

### **Example: Primitive Types**
Primitive types are compatible if they match exactly.

```typescript
let num: number = 42;
let anotherNum: number = num; // Compatible

let str: string = "Hello";
// str = num; // Error: Type 'number' is not assignable to type 'string'
```

### **Example: Object Compatibility**
Object types are compatible if the source type has all the properties required by the target type.

```typescript
interface Named {
    name: string;
}

let person = { name: "Alice", age: 30 };
let namedPerson: Named = person; // Compatible because 'person' has 'name'
```

---

## **2. Function Compatibility**

Functions are compatible if their parameters and return types are compatible.

### **Parameter Compatibility**
- A function can have more parameters than required, but not fewer.
```typescript
type Func = (a: number, b: number) => void;

const fn1: Func = (a, b) => console.log(a + b); // Compatible
const fn2: Func = (a) => console.log(a); // Error: Parameter 'b' is missing
```

### **Return Type Compatibility**
- Return types must be compatible for the function to be assignable.
```typescript
type Add = (a: number, b: number) => number;

const adder: Add = (a, b) => a + b; // Compatible
const wrongAdder: Add = (a, b) => `${a + b}`; // Error: Type 'string' is not assignable to type 'number'
```

---

## **3. Interface and Class Compatibility**

### **Interface Compatibility**
Two interfaces are compatible if one has a superset of the properties of the other.

```typescript
interface Bird {
    wings: number;
}

interface Sparrow extends Bird {
    chirp: () => void;
}

let bird: Bird = { wings: 2 };
let sparrow: Sparrow = { wings: 2, chirp: () => console.log("Chirp") };

bird = sparrow; // Compatible
// sparrow = bird; // Error: Property 'chirp' is missing
```

### **Class Compatibility**
Class instances are compatible based on their structure, not their declarations.

```typescript
class Point {
    x: number;
    y: number;
}

class ColoredPoint {
    x: number;
    y: number;
    color: string;
}

let point: Point = new Point();
let coloredPoint: ColoredPoint = { x: 10, y: 20, color: "red" };

point = coloredPoint; // Compatible
// coloredPoint = point; // Error: Property 'color' is missing
```

---

## **4. Covariance and Contravariance**

### **Covariance (Output)**
Return types can be a subtype of the target type.

```typescript
interface Animal {
    name: string;
}

interface Dog extends Animal {
    breed: string;
}

let animalGetter: () => Animal;
let dogGetter: () => Dog;

animalGetter = dogGetter; // Compatible (Covariant)
```

### **Contravariance (Input)**
Parameter types can be a supertype of the target type.

```typescript
let printAnimal: (animal: Animal) => void;
let printDog: (dog: Dog) => void;

printDog = printAnimal; // Compatible (Contravariant)
```

---

## **5. Generics Compatibility**

Generics in TypeScript are compatible if their structure aligns.

### **Example: Simple Generic Compatibility**
```typescript
interface Box<T> {
    value: T;
}

let stringBox: Box<string> = { value: "Hello" };
let numberBox: Box<number> = { value: 42 };

// stringBox = numberBox; // Error: Type 'Box<number>' is not assignable to type 'Box<string>'
```

### **Example: Covariance with Generics**
```typescript
interface Container<T> {
    contents: T;
}

let animalContainer: Container<Animal>;
let dogContainer: Container<Dog>;

// animalContainer = dogContainer; // Error: Type 'Dog' is not assignable to type 'Animal'
```

---

## **6. Union and Intersection Compatibility**

### **Union Compatibility**
Union types are compatible if the value matches one of the types in the union.

```typescript
type StringOrNumber = string | number;

let value: StringOrNumber;
value = 42; // Compatible
value = "Hello"; // Compatible
// value = true; // Error: Type 'boolean' is not assignable
```

### **Intersection Compatibility**
Intersection types require the value to satisfy all combined types.

```typescript
type Named = { name: string };
type Age = { age: number };

type Person = Named & Age;

const person: Person = { name: "Alice", age: 30 }; // Compatible
```

---

## **7. Special Cases**

### **Compatibility with `any`**
The `any` type is compatible with all types.

```typescript
let value: any;
value = 42; // Compatible
value = "Hello"; // Compatible
```

### **Compatibility with `unknown`**
The `unknown` type is assignable to no types except `unknown` and `any`.

```typescript
let unknownValue: unknown;
// let str: string = unknownValue; // Error: Type 'unknown' is not assignable to type 'string'

unknownValue = "Hello"; // Compatible
```

### **Compatibility with `never`**
The `never` type is not assignable to any type, but all types are assignable to `never`.

```typescript
let neverValue: never;
// neverValue = 42; // Error: Type '42' is not assignable to type 'never'
```

---

## **Interview Questions**

### **Basic Questions**
1. **What is structural typing in TypeScript?**  
   **Answer**: Structural typing determines compatibility based on the structure of types rather than their declarations.

2. **How does TypeScript handle compatibility between two objects?**  
   **Answer**: An object is compatible if it has all the required properties of the target type, even if it has additional properties.

---

### **Advanced Questions**
1. **What is covariance and contravariance in TypeScript?**  
   **Answer**:  
   - Covariance applies to output types (e.g., return types can be a subtype).  
   - Contravariance applies to input types (e.g., parameter types can be a supertype).

2. **How does compatibility differ between `any` and `unknown`?**  
   **Answer**:  
   - `any` is compatible with all types and can be assigned to or from any type.  
   - `unknown` is not assignable to other types without explicit narrowing or type assertion.

3. **What happens when assigning a union type to another type?**  
   **Answer**: The assignment is valid if the value matches one of the types in the union.

---

### **Key Takeaways**
1. TypeScript uses structural typing for type compatibility.
2. Functions are compatible if their parameters and return types align.
3. Top types (`any`, `unknown`) and bottom types (`never`) behave differently in compatibility scenarios.
