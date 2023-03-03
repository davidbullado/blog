# Ending Dependency Chaos: A Proposal for Comprehensive Function Versioning

Have you ever experienced starting a project, and 6 months later attempting to update your dependencies, only to find a deluge of updates? 
Or turning on your machine that has been off for 6 months and having to wait 20 minutes for the system to finish updating? 
Or frantically updating your Nvidia driver without noticing any difference?

## TL;DR
> This article proposes a new approach to versioning systems to address the problem of constantly updating dependencies. This approach involves versioning every exported function using a format like `function-name.a.b.c`, where a is the major number, b is an identifier for the file (or class / module), and c is the function's version increment. This approach offers benefits such as providing a more precise way to track changes and updates to dependencies, reducing the risk of breaking changes caused by dependency updates, and improving the security of projects. The article suggests that this approach has the potential to enhance the efficiency, reliability, and security of software development and is worth exploring further.

### The traditional approach to versioning systems
When we think of versioning systems, we often imagine it as: `1.2.3` with a *major*, *minor*, and *increment number*.

Let's put ourselves in the shoes of a developer of a heavily used library. As we are very conscientious, we regularly update our packages to patch any potential security vulnerabilities.

Let's say our project has 203 dependencies, which is the average number of dependencies according to [GitHub](https://github.blog/2020-08-04-secure-at-every-step-how-githubs-dependency-graph-is-generated/).

What happens if even one dependency is updated? No problem, I update my package.json (or equivalent), and most importantly, I increment the last version number of my project, for example, I go from `1.2.3` to `1.2.4`. I commit, push, and deploy my project, and come back the next day only to find another one of the 203 dependencies has been updated again (it went from version `10.2.123` to `10.2.124`).

### The problem of constantly updating dependencies


Perfect, right? But perhaps you are starting to see the problem. What happens if I want my dependencies to be constantly updated? Well, with 203 dependencies, which themselves contain an incalculable number of dependencies, which in turn contain an incalculable number of dependencies...

In summary, developers should spend their days updating the versions of their packages, publishing them, and so on. It only takes one dependency to be updated for this to trigger a cascade of updates. In economics, we would call this monetary inflation. Here, it is an inflation of version numbers.

### Proposal for comprehensive function versioning

Including a dependency should be like entering into a **contract**. I use function X, which takes A as an argument and returns B. If function Y is updated, why should I update my project?

The solution I propose would be to *version absolutely every exported function*. Each function could be identified by **function-name.a.b.c**, with a being the major number, b an identifier for the file, and c the function's version increment.

### Example implementation in TypeScript

`operators.ts`:
```js
@moduleVersion 2
 ...
@funcVersion 16
export function addFunction(a: number, b: number): string {
  return a + b;
}
```

Then, in the `package.json` that imports this library, there would be something like this:
```js
  "dependencies": {
    "my-math-lib": {
        "addFunction": "1.2.16"
    },
   }
```

Let's imagine several scenarios:

- I want to modify my addFunction function to introduce error handling. No problem! I increment `@funcVersion 16` to` @funcVersion 17`. The version identifier becomes: `"addFunction": 1.2.17`
- My modification will impact all functions in my file or class. No worries, I increment `@moduleVersion` to 3. The version identifier becomes: `"addFunction": "1.3.17`
- I have refactored my logging system that impacts my entire project. Alright, `@globalVersion` goes up to 2.  The version identifier becomes: `"addFunction": "2.3.17`

### The benefits
This approach offers several benefits for developers.
- First, it provides a more granular and precise way to track changes and updates to dependencies, allowing developers to more easily identify and fix issues related to specific functions or modules.
- Second, it can reduce the risk of breaking changes caused by dependency updates, as developers can explicitly specify the version of each function they use and test it before updating.
- Third, it can improve the security of projects by making it easier to identify and patch vulnerabilities in dependencies.
- Finally, it can streamline the development process by reducing the need for massive and time-consuming updates of multiple dependencies, allowing developers to focus on more important tasks. Overall, the proposal for comprehensive function versioning has the potential to enhance the efficiency, reliability, and security of software development, and is worth exploring further.

### Final thoughts
Then, there's nothing stopping us from making this versioning system completely automated. Any IDE could detect a modification and suggest a version increment. Would this put an end to update inflation? Or add another layer of uncesssary complexity in the development process ? 
