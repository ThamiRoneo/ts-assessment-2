# TypeScript Exercises Reflection

## What does a generic constraint (`K extends keyof T`) buy you over `any`?

Using `K extends keyof T` lets TypeScript keep track of which keys actually exist on an object. Instead of allowing any random string, it limits the key to the real keys of `T`.

That means `obj[key]` keeps its proper type as `T[K]`. For example, if the key is `"price"`, TypeScript knows the result is a number. If the key is `"name"`, it knows the result is a string. With `any`, all of that useful information disappears.

The biggest benefit is that mistakes are caught earlier. If I try to access a key that does not exist, TypeScript gives me a compile-time error instead of letting the bug turn into a runtime problem. It also improves autocomplete and makes the code easier to work with because the editor understands the shape of the object.

## When would you use a mapped type vs a utility type like `Pick`?

I would use a utility type like `Pick` when I only need a common, straightforward transformation. For example, if I want a type with just a few properties from another type, `Pick<Product, "id" | "name">` is simple and clear.

Mapped types are better when I need more control over how the type is transformed. They let me loop over keys and change the properties in a custom way, such as making every field nullable, readonly, optional, or even renaming keys with template literal types.

So, for common operations, utility types are usually the cleaner choice. For more specific or domain-specific transformations, mapped types are more flexible.

## What is the difference between `unknown` and `any`, and why is a type guard safer than a cast?

`any` basically turns off TypeScript checking for that value. Once something is `any`, I can call methods on it, access properties, or pass it around without the compiler warning me. That can be convenient, but it also removes the safety TypeScript is supposed to give.

`unknown` is safer because it forces me to check the value before using it. If I receive an `unknown`, TypeScript will not let me treat it like a string, object, or number until I prove what it is.

A type guard is safer than a cast because a type guard actually checks the value at runtime. For example, an `isUser` function can verify that the value has the fields a `User` should have. A cast like `value as User` only tells the compiler to trust me. It does not verify anything, so it can hide bugs and create false confidence.

## How does the `never` exhaustiveness check in the reducer protect you?

The `never` exhaustiveness check makes sure every possible action in a union has been handled. In a reducer, this is useful because actions usually grow over time as new features are added.

When all action types are covered in a `switch`, the remaining value in the default case should be `never`. If I add a new action later and forget to handle it, TypeScript will complain because the action is no longer `never`.

That turns a possible runtime bug into a compile-time warning. Instead of silently ignoring a new action, the reducer forces me to update the logic properly. It makes the code easier to maintain and safer to extend.
