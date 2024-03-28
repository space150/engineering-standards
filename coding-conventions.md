[back to table of contents](readme.md)

(*this document is part of the space150 Engineering Standards & Guidelines*)

# Coding Conventions

## Overview
Within this section, we enter what is likely to be one of the most debated, and ever-evolving, portions of our standardization documentation. Therefore it is imperative we establish what we are trying to accomplish with our standards.

Many conventions and best development practices carry over from language to language and framework to framework. These "conventions" are the type of thing we strive to align on and document as standards. This said, it is impossible to stay completely language or framework agnostic. Today, JavaScript is our most universally utilized language at space150. That may change in the future, but because of its ubiquitous use, we'll try to utilize JavaScript with TypeScript throughout this section for actual code examples, but this is to be narrative only and should be able to carry over to other languages. Where necessary, we may call out a different use case for a language or framework for documentation purposes.

## Naming Conventions
The ultimate goal of our naming conventions is to ensure that code is descriptive and clear to new readers. No matter the type of identifier (function, variable, etc) the name should always be explanatory. Programmers should avoid abbreviations whether it is in a class, function, or variable name. Poorly written names create maintenance headaches and increase business costs. This convention of explicit readability dates back to early programming and is often referred to as ["magic numbers"](https://en.wikipedia.org/wiki/Magic_number_(programming)).

At space150, our standard is to use a natural language style for our naming. It should be neither too short nor too verbose, but rather explanatory.

#### **A good example of an explanatory function name:**

```Typescript
function saveGuestBookEntry(guestBookEntry:string) {
  // some save logic would exist here
  console.log('Saved, ' + guestBookEntry) 
}
```

## Identifier Naming
The following reference guide for identifier naming conventions should be followed for easy recognization by other engineers. TypeScript is always a recommended framework with JavaScript so that we have declarative syntax.

`UpperCamelCase` - class / interface / type / enum / decorator / type parameters

`lowerCamelCase` - variable / parameter / function / method / property / module alias

`lower-hyphenated` - Folder names

`CONSTANT_CASE` - global constant values, including enum values

## Functions
One of the fundamental rules of writing a good function is to ensure it does one thing. What does this mean? A function can generally execute and do any number of things. When a function has more concerns than doing one thing it becomes less readable and hence more difficult to maintain. Our standard is single concern functions when at all possible. Try to make your functions [Pure](https://en.wikipedia.org/wiki/Pure_function) if possible.

When naming functions be sure to keep in mind the top-level rules of our ["naming conventions"](coding-conventions.md#naming-conventions).


### Prefer lamda syntax
It is preferred to use ES6 lambda syntax when defining functions, as this can help prevent tricky `this` reference issues in classes.

**Examples:**

```Typescript
class MyClass {
  name:string = 'Billy'

  myFunction = ():string => {
    return 'Hello, ' + this.name + '!' // Correctly resolves `this` reference 100% of the time
  }
}
```

```Typescript
const myFunction = ():string => {
  return 'Hello world!'
}
```

### Fully type method signatures
When creating functions, there should always be a defined input data type and a defined output data type. Even in trivial cases like below, we should explicitly define the input and output data types; this gives us clarity when reading (especially in longer functions where the return cannot be immediately seen) and additionally allows the Typescript compiler to easily show errors when a type mismatch occurs on either the input or output.

```Typescript
const myFunction = (input:string):string => { // Good. Defined input and output data structures
  return 'Hello world, ' + input + '!'
}
```

```Typescript
const myFunction = (input):string => { // Bad. Implicit `any` on the input argument.
  return 'Hello world, ' + input + '!'
}
```

```Typescript
const myFunction = (input:any):string => { // Bad. Explicit `any` on the input argument is to be avoided. If `any` must be used, the expectation is that the function heavily interrogates the input at runtime to make sure that the data the function requires is a valid structure.
  return 'Hello world, ' + input + '!'
}
```

```Typescript
const myFunction = (input:string) => { // Bad. Implicit `any` on the function output.
  return 'Hello world, ' + input + '!'
}
```


## Code Comments
Commenting your code can provide additional context for future maintenance. The following list of dos and dont's serve as our guidelines for useful comments in a project.

### **Dos**
- Do use clarification comments to provide extra helpful information on difficult-to-follow logic. Often clarification comments are an indicator of a follow-up needed, and they provide useful information for future maintenance
- Do provide links to original references of code if borrowed (ensure to check the licensing on code to ensure it is acceptable). Citing a StackOverflow solution is a common occurance.
- Do provide bug specific comments for otherwise unnecessary logic:

    ```html
    <div> <!-- This extra empty div here is to fix a Safari specific bug with -webkit-sticky -->
    </div>
    ```

### **Dont's**
- Don't use superfluous or redundant comments when the variable/function names themselves contain the information we are trying to convey.

    ```Typescript
    // Save the post
    const savedPost:GuestEntry = saveGuestBookEntry("Hello")
    ```
- Don't comment out code and leave it in a commit; this is what source control is for. We can always go back in the Git history and find what the old code was.
- Don't leave hardcoded tests in the codebase. (This is how a password or secret ends up getting committed into a project)
    ```Typescript
    // const client:APIClient = createAPI('12345-api-key-67890`) // Bad. Do not hardcode secrets, even commented out ones!
    const client:APIClient = createAPI(process.env.API_KEY) // Good. Always pull secrets from secure locations not committed to the repository.
    ```
- Don't use a comment to highlight a shortcut taken in code `// HACK: I was rushed here, I'll redo this later, sorry mom`. A more amenable solution is to write a comment saying that this is currently working, but could be improved: `// This algorithm will be fine as long as the object has <100 items`.

## Types
Wherever possible, define and use some form of type definitions (Typescript `interface` and `type` declarations, `zod` schemas, or both!) to describe data structures in the application. This allows other developers to check at a glance what the inputs and outputs are throughout the program, and also helps the compiler flag bad inputs or outputs.

### **Dos**
- Create interfaces and types for data structures
- Try to keep your data type definitions close to the source of the structures. For example, if there is an `parseInput` function that has custom inputs & outputs associated with it, define those inputs and outputs immediately preceding the function declaration. If you have a large `API`-like class, define the inputs and outputs of the API right alongside it.

    ```Typescript
    export enum SetupOption {
      Off,
      Partial,
      On,
    }

    export const parseSetupOption = (input:string):SetupOption {
      switch (input.toLowerCase()) {
        case 'off':
          return SetupOption.Off
        case 'partial':
          return SetupOption.Partial
        case 'on':
          return SetupOption.On
        default:
          throw new Error('Unrecognized setup option!')
      }
    }
    ```

    ```Typescript
    export interface PutDataInput { ... }
    export interface PutDataOutput { ... }

    export class MyAPI {
      putData: (input:PutDataInput):Promise<PutDataOutput> => {
        ...
      }
    }
    ```

- On TypeScript projects, use `zod` schemas to define your data structures, especially if the data structure contains user input. Zod schemas have a `safeParse` function that not only validates that the data meets the schema, but also provides user-friendly error messages about what pieces of the data were invalid.
- Prefer libraries that have TypeScript definitions included in them, or have an `@types/` package available.
- Prefer the ESM style of `import x from 'y'` instead of the CommonJS `const x = require('y')` when possible, even if you are having TypeScript output to CommonJS.

### **Don'ts**
- Avoid a single giant `./src/types.ts` at the root of your project that includes all of the types across the entire project. Instead, try to keep type definitions close to where they are created.
- Avoid using implicit and explicit `any` declarations, as this makes it difficult to see at a glance what data is being operated on.

## Consistency
For any style question that isn't settled definitively by this specification, follow what other code in the same project is already doing (aka, be consistent).

- We generally prefer semicolons to only be used where they are strictly necessary, instead of at the end of every statement.
- We generally prefer using single-quotes `'` instead of double-quotes `"` for strings.
- We generally prefer using `2-spaces` instead of tabs.
- Avoid wrapping object property keys in quotes if they are not needed. Instead of `{ 'key':'value' }`, use `{ key:'value'}`.
- In variable names & object properties, we generally prefer using `camelCase` property names. For URL paths, we prefer `hyphens` to separate words. We tend to use `underscores` only in environment variable names.
- We tend to avoid the use of linters & formatters (ESLint, etc), as we've felt too constrained by their output. If you do want to use them, make sure your project does not require them to be installed globally, as different projects may have different standards than yours.

Project-specific standards always override these global preferences. Consider adding project-level workspace settings in VSCode to give suggestions on how code should be formatted in your project.