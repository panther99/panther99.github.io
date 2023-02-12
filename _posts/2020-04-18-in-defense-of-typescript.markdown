---
layout: post
title: "In defense of TypeScript"
date: 2020-04-18 13:58:52 +0200
tags: typescript javascript development
categories: software-development
---

I know what you may think now. _Here we go, yet another article telling us how great TypeScript is._ Why do we need to defend a language backed by Microsoft? With typings available for pretty much every popular NPM package out there? And from whom specifically? If we take a look at results of StackOverflow developer surveys for the last three years ([2017](https://insights.stackoverflow.com/survey/2017#most-loved-dreaded-and-wanted), [2018](https://insights.stackoverflow.com/survey/2018#most-loved-dreaded-and-wanted), [2019](https://insights.stackoverflow.com/survey/2019#most-loved-dreaded-and-wanted)), we can see that TypeScript was always in the four most loved programming languages.

While it's true that TypeScript is very popular and loved by developers all around the world, we still see misconceptions about it every day. Look - it's okay if you find TypeScript off-putting or you just don't need it. I never had to use Python in my workflow and I don't like it, but I see why people would use it. So, why do we need to talk about TypeScript?

## JavaScript ecosystem has evolved

Not so long ago, JavaScript was a little more than language for showing fancy animations on the web. Nowadays, beside being used on the web, JavaScript is used for writing desktop applications (Electron), server-side applications (Node.js) and even for the IoT. Currently, there are over 1 230 000 packages on the NPM (data by [modulecounts](http://www.modulecounts.com/)). There are many courses, tutorials and jobs for JavaScript developers out there. All in all, knowing JavaScript today is a huge advantage. This is true even if you don't plan using it as your primary language.

But things are not so great as they may look at first. We all know about [node_modules jokes](https://www.google.com/search?q=node_modules+memes&source=lnms&tbm=isch&sa=X&ved=2ahUKEwjIr--l4a7oAhUy_SoKHXHFC1MQ_AUoAXoECAsQAw&biw=1920&bih=897). With so much popularity and easiness of learning comes a bad code too. We saw that with PHP. This is not a fault of JavaScript, especially not today. As it's often the case, it's about people. Now, there are countless debates whether technology needs to set some limits to prevent the misuse or give it's users free hands. I won't open that discussion in this article. I will just try to address some common misconceptions about TypeScript.

## Misconception #1 - TypeScript has no real purpose 

Who knows how many times I heard this. People asking me why do they need to learn another language which basically boils down to the JavaScript. The truth is - you don't need to. It's quite possible to spend the rest of your career without ever touching any TypeScript code. But my question is why would you do such thing? Why not giving a chance to a tool which can help you to write a better software?

TypeScript definitely has it's purpose and the most important ones are;

* **Improving communication on the team** - JSDoc is helpful, but it isn't that powerful and you need to always check the whole documentation to be sure that information in it is still valid.
* **Helping with refactoring** - trying to change that method to return a different data in JavaScript? Pray it won't break half of your project where that method was used.
* **Preventing you from making dumb mistakes** - adding numbers and strings, no matter how handy can be sometimes, can cause you a great pain in the long run.
* **Allowing you to scale your project easier** - that JavaScript that scales caption you see on the TypeScript's site? Yup, it's true.

Even if TypeScript had no practical purpose, it still wouldn't be a good reason not to learn it. There are some languages (like Scheme, Haskell or Prolog) which may not get you a job[1] but will definitely expand your horizons and help you to become a better developer. Giving the fact that TypeScript is now used by many companies, there is one more reason to at least try it.

## Misconception #2 - TypeScript is hard 

Whether something is hard or not is pretty much subjective. In my opinion, TypeScript won't be hard to a person with solid knowledge of JavaScript. By solid knowledge, I mean you have few JavaScript application running in the production and you understand core subjects (scopes, closures, event loop, prototypes etc). If that's the case, learning TypeScript won't be a problem to you. However, if you didn't have a chance to work with static typed language before (C#, Java, C++ or similar language), it will take some time to get used to it. Again, this shouldn't be an issue because of the _gradual typing_.

Gradual typing allows us to slowly migrate our projects from JavaScript to TypeScript by using `any` in places where we're still not sure about concrete data types. Let's see it on the practical example. Here is a JavaScript method which will fetch badges of the user with specific username.

<script src="https://gist.github.com/panther99/902a6151844875bf5e7b2ff2da2e6634.js"></script>

Let's say we chose to slowly migrate our project from JavaScript to TypeScript and we're turning on `strict` setting in our `tsconfig.json` like we should do. TypeScript will give us a warning for the method above:

![typeerror parameter implicitly any](/assets/2020/04/type-error-parameter-implicit-any.png){: .center-image}

The error above means that we have not explicitly set what type username parameter should have. When type is not set, TypeScript assumes you’d like it’s type to be `any` (which basically means it can be anything). Fortunately, strict option prevents us from shooting ourselves in the foot. Why? Because having implicitly set parameters throughout the project is the surest path to a disaster. As our project grows we will forget about these places and we won’t get the benefits of TypeScript’s compiler analysis. Also, it’s pretty obvious by the parameter’s name what data type we would like it to have.

## Wait a second…

But what about `user` and `badges`? We certainly don’t want to use them as `number`s, `string`s or `boolean`s but as the objects with their respective properties and methods. For now, we will explicitly define them as `any` (even though we’re not required to do so). We will define badges as `any[]` as we know it will return array of some data type. We can also do this for parameters where we’re still not sure what data type they will have. Let’s see our refactored method now.

<script src="https://gist.github.com/panther99/376e143d2f255147dc8d4173114645ed.js"></script>

Now you may ask what makes such a difference between setting something as `any` or `any[]`. Well, it’s certainly better to know that something will be array of some things than some thing (which can be array of some things or who knows what else). But let’s say we want to have a method which will check whether user has any badges:

<script src="https://gist.github.com/panther99/4b579b5bd7f7f8470ad089538d0c625c.js"></script>

As TypeScript knows that `fetchUserBadges` method will return `Promise<any[]>` (a `Promise` which when it’s resolved will return `any[]`), it can give us available properties and methods as we’re writing the method:

![array length property](/assets/2020/04/array-length-property.png){: .center-image}

I know, I know, this is pretty simple example but that’s the whole point – TypeScript on it’s own is _not_ hard. It just takes time to learn how to use it properly, just like it’s the case with any technology out there. Just because you can hack something quickly in the JavaScript doesn’t make it easy. You will still have to learn it’s core concepts in the hard way, by making mistakes and learning from them.

## Misconception #3 – TypeScript slows you down too much

There is something people don’t quite understand when they compare dynamic typed languages with static / strong typed ones. If you ever followed any programming memes page (please don’t if you care about your health), you have probably noticed some image with comparison of Hello world program in Java (C#, C++ or any other static typed language) and in the Python. People who make images like this would like to prove to us how superior Python is. Sadly, they just ruin Python’s image with such lack of understanding some basic things.

Obviously, writing down types does make you slower than not writing them. But that initial work will make you faster in the long run. This means that:

* Debugging will be easier once your project grows
* Navigating code base will be quicker
* You will catch many bugs before the runtime
* Your code will basically document itself (but this doesn’t mean you don’t have to write documentation)

Now, of course, this doesn’t mean you should use TypeScript for every project. Recently I built a simple weather application in TypeScript (I used TypeScript on both, front-end and the back-end) and I wished I did it in JavaScript. But it’s because I only had three routes and three different views on the front-end. TypeScript didn’t help me much there. This is not a fault of TypeScript. It would have many benefits if I chose to extend my application with various services. And / or more complex state management on the front-end.

## Misconception #4 – TypeScript was made by Microsoft, therefore it can’t be good 

Many of us probably know about Microsoft’s dark history. But as someone who hated Microsoft with passion (and still don’t have any sympathies towards it), I can say that Microsoft really changed since Satya Nadella took the position of the CEO, at least with their stance towards open source software. If I can list three great things that Microsoft gave us they would be these ones (in no particular order):

* **C#** - modern language with great support for building safe and robust desktop, server-side and even mobile applications
* **Visual Studio Code** - probably the best open source code editor on the market today with hundreds of thousands extensions and constant improvements in each version (built with TypeScript)
* **TypeScript** - do I need to say more?

Saying that TypeScript is bad because it was made by Microsoft is childish at best. We may not like Microsoft’s business strategy but we need to remember that there are thousand of workers in the Microsoft who do their best to build amazing products and TypeScript is definitely one of them.

## Misconception #5 – TypeScript is hard to read

Another variation of the misconception #2. When people say that TypeScript is hard to read, they often mean that projects they’re trying to contribute to or libraries they’re using are hard to read. This is understandable, considering how complex types can become in a TypeScript codebase.

But guess what? TypeScript is not harder to read than any other strong typed language. It’s about inherent complexity of the project you’re trying to dive in and if you ever worked on production-level apps you know they can grow very quickly. Even in smaller applications type definitions could be long and tedious to read. I remember when I wanted to add types to the `Object.entries` method. I ended up writing something like this:

```ts
export const typedObjectEntries = Object.entries as (
    o: T,
) => Array<[Extract<keyof T, string>, T[keyof T]]>;
```

And I ended up calling it whenever I was using `Object.entries` in the project (and this was before I knew I should import `es2017`). I know what it does, it’s well named and there is no reason to make it simpler just because someone will spend a bit more time reading it. If it’s not so obvious what the method does, you can always add documentation for it. Writing unreadable code is bad, but simplifying code where you shouldn’t could be much worse. Einstein has famously said:

> Everything should be made as simple as possible, but not simpler than that.

So, when the code looks hard to read, ask yourself a question. Is it possible for me to simplify this without affecting it's intent? Would simplifying this code bring any benefits or it can lead to bigger confusion? Don't think about problems like these through the prism of the programming language. Always try to solve a problem by applying general solutions, and later customize it to follow idioms of the language.

## Conclusion

As it's often the case, this article sums up my experiences of working with TypeScript and in no way tries to pose as a list of empirical facts. I hope it will at least help some people to reconsider their decision to reject using TypeScript in their workflow. Some will disagree with the opinions I expressed here for sure. And that's okay. I may be biased as an advocate of static typed languages, but I would really like to hear your opinions. I also hope this won't cause another heated debate. The goal of this article is to question the most common misconceptions about TypeScript, not to bash JavaScript (which, at the end, TypeScript _is_ mostly).

[1] This doesn't mean you can't find jobs for Haskell or Prolog developers. I think Scheme is used pretty much only on the university, but Clojure is a variant of Lisp which is used more for commercial projects.
