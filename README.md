# 1. What’s a closure? Where in the code is there a closure?

```
A closure is when a function "remembers" the variables from its outer scope even after that scope has finished executing. It's useful for maintaining state in JavaScript.
```
```
In the code, an example of closure can be found in the computed properties like paginatedJokes. It relies on sortedJokes, which in turn depends on sortOption and jokes. Even though these values might change over time, the computed function "remembers" their previous states, keeping everything reactive.
```

# 2. Which are the potential side effects in any function? Could you point out any of these cases in your code? Are they expected? Can they be avoided?

```
Side effects happen when a function modifies something outside its own scope, like updating a global variable, making an API request, or changing the DOM.
```
```
In this case, fetchJokes() and fetchCategories() both introduce side effects because they fetch data from an external API and update the component’s state (this.jokes and this.categories). These side effects are expected and necessary for the app to work.
```
```
To manage them properly, we handle errors and ensure that state updates happen within Vue’s reactivity system. If needed, we could also use Vue’s lifecycle hooks or external state management tools to have better control over these effects.
```# vue-jokes-challenge
# vue-jokes-challenge
