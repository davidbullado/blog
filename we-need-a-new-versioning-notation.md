# Ending Dependency Chaos: A Proposal for Comprehensive Function Versioning

Have you ever experienced starting a project, and 6 months later attempting to update your dependencies, only to find a deluge of updates? Or turning on your machine that has been off for 6 months and having to wait 20 minutes for the system to finish updating?

### The traditional approach to versioning systems
When we think of versioning systems, we often imagine it as: 1.2.3 with a major, minor, and increment number.

Let's put ourselves in the shoes of a developer of a heavily used library. As we are very conscientious, we regularly update our packages to patch any potential security vulnerabilities.

### The problem of constantly updating dependencies
Let's say our project has 203 dependencies, which is the average number of dependencies according to GitHub (https://github.blog/2020-08-04-secure-at-every-step-how-githubs-dependency-graph-is-generated/).

What happens if even one dependency is updated? No problem, I update my package.json (or equivalent), and most importantly, I increment the last version number of my project, for example, I go from 1.2.3 to 1.2.4.

Perfect, right? But perhaps you are starting to see the problem. What happens if I want my dependencies to be constantly updated? Well, with 203 dependencies, which themselves contain an incalculable number of dependencies, which in turn contain an incalculable number of dependencies...

In summary, developers should spend their days updating the versions of their packages, publishing them, and so on. It only takes one dependency to be updated for this to trigger a cascade of updates. In economics, we would call this monetary inflation. Here, it is an inflation of version numbers.

### Proposal for comprehensive function versioning

Including a dependency should be like entering into a **contract**. I use function X, which takes A as an argument and returns B. If function Y is updated, why should I update my project?

The solution I propose would be to version absolutely every exported function. Each function could be identified by function-name.a.b.c, with a being the major number, b an identifier for the file, and c the function's version increment.

### Example implementation in JavaScript

```js
@fileVersion 2
 ...
@funcVersion 16
export function addFunction(a: number, b: number): string {
  return a + b;
}
```

Then, in the package.json that imports this library, there would be something like this:

```js
  "dependencies": {
    "my-math-lib": {
        "addFunction.1.2": 16
    },
   }
```
