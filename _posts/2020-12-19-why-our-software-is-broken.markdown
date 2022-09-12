---
layout: post
title: "Why our software is broken? "
date: 2020-12-19 14:08:47 +0200
comments: true
tags: software-design
categories: software-development
---

Long time no see! Since May when I wrote my last post, many things happened in my life. I started living alone, moved to a new city and found a new job. I think I had pretty good rest from the writing. Today, I’ll write about a topic which interests us all; C-level staff, developers, managers, end users… Recently, I watched an [amazing talk](https://www.youtube.com/watch?v=rmueBVrLKcY) by Joe Armstrong in which he presented quite interesting and a bit controversial thought;

> *Computer science is confusing because it’s not a science and there are too many ideas floating around*.

I sincerely believe this can explain lot of issues in software development currently. We’re constantly trying to improve quality of our software, lower the time it takes to build it and prevent bugs. When confronted with the question how to build a quality software, different people will give you different answers. Some hardcore FP enthusiasts will try to persuade you to rewrite whole your back-end in Haskell. That person in the corner will swear by TDD. A manager on the third floor will say we should ditch agile and use waterfall.

Most people are missing a huge factor here; *a failure to reduce complexity*.

## Personal experience

In the company I worked in the past, we received a new project which had to be done in React. At that time I had a bit of experience with React and understood basic ideas behind it. There was another colleague on that project who almost never worked with React before. Right at the start, we received a task to create an image configurator. On the surface, it was basic; you could choose colors, icons, text font and other stuff. What made it quite complex was a bad design. Instead of using states, my colleague (understandably, since he worked mostly with jQuery before) did direct mutations on the DOM. If you worked with React seriously, you know this is a big *no-no*.

Unfortunately, this part grew and became harder and harder to maintain, to the point where we spent half an hour fixing some trivial issue. This was the first time in my career where I saw how important it is to try to reduce complexity as much as you can. Sometimes we tend to go too far when we’re solving particular problem. Always keep in mind that you’re writing for other people first, and then for the machine. Machine won’t maintain your code; you’re the one who will have to explain it to others. Even if you leave the company before that, don’t do something you won’t like to experience yourself.

## Alright, but how can I reduce complexity?

Now, there is a catch. Unfortunately, it’s possible to reduce complexity of your code only to **some** degree. Software is inherently complex on it’s own. It has to take inputs, handle edge cases and errors, work with files and data received over the network, draw things on the screen and many other stuff. We’re developing on the machines which are dozens of times more powerful than the ones used fifty years ago. On top of that, you have deadlines, poorly documented libraries, legacy code etc. It’s understandable why software development today is much more challenging in many ways.

* **Don’t try to appear smart**. If there is a simple way to solve particular problem which works and is easy to understand, use it. Don’t build unnecessary abstractions and features no one asked for. Use descriptive variable and method names. It’s much smarter to write code which is easy to understand and maintain than something which only you will understand.
* **Write documentation**. Even *you* won’t be able to understand some parts of your code after some period of time. Describing what some part of your code does will help other people working on your code and it will help you later as well.
* **Write tests.** Sometimes, writing a test before you start implementing certain feature will help you to better design it.
* **Ask yourself do you really need it.** I can’t find that article now, but I think it was written by Joel Spolsky, ex-CEO of StackOverflow. I use it as my guiding star and I think about it whenever I’m starting to work on a new side project. In essence, it says this; your job is not to write the code, but to solve problems. Ideally, you should solve them without writing any code. Most of the time it’s not possible though, so you should strive to write least possible amount of it which will be both, well working and understandable.

There are many other ways but these are some of the most important ones. Remember, it’s always about balancing. Adding new features will definitely make a whole system more complex, but that doesn’t mean we shouldn’t add new features at all. It means we should be more careful when doing so, taking into account the most important things; value it gives us (both in the short-term and long-term) as well as the effort it takes to be built and maintained.

## Things should be as simple as they can be, but not simpler than that

I’m using this Einstein’s quote fairly often when I’m talking about software development. It’s so amazing because it can be applied across many different fields. Reducing complexity in the project shouldn’t mean oversimplifying stuff. As I said, software is inherently complex on it’s own; trying to simplify something too much will have the opposite effect. For example, if you’re building a web application similar to [Lichess](https://lichess.org/), you’ll have to think about lots of things:

* fault tolerance
* handling hundreds of thousands of concurrent users
* streaming
* security

And so on and so on. As long as you can have controlled complexity in the project, it’s okay for it to exist in the project. Controlling complexity means building a flow which will help new developers to learn how the system works so they can make contributions to it. Most obvious tool for that job is writing documentation. In an article *The Plea for Lean Software* written by Niklaus Wirth, creator of Pascal, he says something which was proven to be true more than two decades later:

> *The belief that complex systems require armies of designers and programmers is wrong. A system that is not understood in its entirety, or at least to a significant degree of detail by a single individual, should probably not be built.*

To sum up; it’s okay for some things to be complex. What you should prevent though is for things to become so complex that adding each new feature will take significant amount of time. This will make maintenance of the software painful and lead to even higher complexity in the future. The initial effort of implementing a feature is not as important as the effort of maintaining it.

## Conclusion

I intentionally made this article smaller than I planned to. My goal was to motivate you to think about this more. We’re witnessing enormous expansion of the software and I believe this topic will be even more relevant in the future. I’ll leave you to think more about this on your own and to do your own research on this problem. Before that, I’ll recommend you an amazing book I recently read; [Code Simplicity: The Fundamentals of Software](https://www.amazon.com/Code-Simplicity-Fundamentals-Max-Kanat-Alexander-ebook/dp/B007NZU848) by Max Kanat. It’s only 80 pages long but contains lot of valuable information on reducing software complexity and overall proper software design. I’ll suggest you to read it and take notes. No matter which technology you use or how your projects will look, these points will be helpful to you for the rest of your career.