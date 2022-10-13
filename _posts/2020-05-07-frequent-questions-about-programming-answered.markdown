---
layout: post
title:  "Frequent questions about programming answered"
date:   2020-05-07 15:53:27 +0200
tags: advices career beginners index
categories: software-development
---

With the growth of elitism and fanboyism in software development world, as well as many questions I noticed all over the web showing fundamental misunderstanding about this topic, I felt the urge to write this article. You’re free to contribute and add your own opinions. Goal of this article is to answer the most frequently asked questions about software development.

You’re completely free to share this article or parts of them as long as you share the source too. I will occasionally update it with new questions and answers. Questions have quite mixed up order, but I hope you'll be able to find the answers you need. I will try to organize them better in the future.

Initial version of this article was written on Quora so you will see few links to the answers from Quora.

&nbsp;

## What kind of equipment do I need to start with programming?

Any decent computer will do the job.

See: [Garry Taylor's answer to What equipment do you think is the most important for being great a programmer?](https://www.quora.com/What-equipment-do-you-think-is-the-most-important-for-being-great-a-programmer/answer/Garry-Taylor-5)

&nbsp;

## Which programming language should I learn first?

In my opinion, your first language should be:

* easy to learn
* have a strong community so you can get your answers easily
* a language with many jobs available to you

The most obvious choices in that case are Python, Ruby, JavaScript or PHP. All of them are easy to learn, have strong community behind them and there is a plethora of jobs for them.

&nbsp;

## Do I need to be a great mathematician to become a programmer?

It depends on your field of work. For most of the things (web, desktop and mobile development), strong math background is not required. Exceptions are game development (but you can develop simple games without strong math knowledge), machine learning, data science, cryptography etc. Even then you will most probably need knowledge in some specific fields of math.

See: [What areas of computer science/programming/software development require a good math background?](https://www.quora.com/What-areas-of-computer-science-programming-software-development-require-a-good-math-background)

&nbsp;

## Do I need to know X text editor / IDE to become a great programmer?

No. This is one of those annoying polemics where some people love to think highly of themselves because they use certain tools in development. Use whatever allows you to write applications. It doesn’t matter if it’s Sublime, Atom, TextMate, Vim, Emacs or anything else capable of editing source code.

This is, of course, exception for platform-specific developers; for example, if you’re developing for Windows it’s recommended to be familiar with Visual Studio and, if you developer for iOS / Mac, it’s recommended to be familiar with XCode (and most of the time, you have no other choices). For everything else, it doesn’t really matter.

&nbsp;

## I just graduated from the university, how can I find a job?

Finding a first job in the industry can be quite hard. Don't worry though - finding a job is easier than it was in the past.

The first thing you should do after graduation (if you haven't already) is to build a good portfolio. No one will expect it to be amazing so it's enough for you to have few projects you can show you to the potential employers.

If you don't have any idea, here are some nice lists of project ideas you can try to implement:

* [A list of practical projects that anyone can solve in any programming language](https://github.com/karan/Projects)
* [A collection of application ideas which can be used to improve your coding skills](https://github.com/florinpop17/app-ideas)
* [A curated list of project-based tutorials](https://github.com/tuvtran/project-based-learning)

Post your projects on the GitHub and attach the link to your GitHub profile in your CV. Google for the software development job boards in your country and look for the internships or the junior developer jobs. Don't worry if you don't know all the things listed on the job post. Be persistent and eventually you'll land the job.

&nbsp;

## Is Java slow?

No. Right now we’re in 2020, not in 1995. JVM is developed by some really badass software engineers who know what they’re doing much better than those who call languages slow. If your application is slow, shame on you, it’s not the problem of Java (or you’re trying to do something for which Java isn’t made for, like, developing OS in Java, which, again, is not the problem of Java itself).

&nbsp;

## Is Java dead?

It looks like it’s dying last 22 years. It’s kinda hard to kill someone who owns the top place for decades.

Again, no.

&nbsp;

## What’s the best programming language ever?

This is like asking what’s the best tool ever. Seriously, do you really think that hammer is better than a screwdriver or a saw? Same thing with languages.

If language X is suitable for your project, use it. If you don’t know it, see if there are other languages suitable for your project which you already know.

In case you find the more suitable language you already know, use it. If there is no such language, learn one which is suitable for your project.

&nbsp;

## Is X programming language dead?

If there is need for developers to maintain projects written in X programming language, that language is not dead. Cobol was invented almost sixty years ago and it’s still relevant and used by many institutions (this doesn't mean it's a good idea to learn it today though).

&nbsp;

## Is X programming language bad?

Language can have some weird design choices of authors or it can have some quirks and weird behaviour (most notable ones are PHP and JavaScript), but if it does the job well, use it. If it’s bad, learn how to use it in your favor and try to avoid bad practices.

If you have no choice (e.g. JavaScript on front-end), then you need to use it.

&nbsp;

## I learned X programming language, what's the next I language I should learn?

Stop.

First, you haven’t learned A programming language. There is no way for someone to completely learn any programming language. Even if you managed to remember all of it’s keywords and the whole standard library, you still haven’t learned it.

No one cares how many programming languages do you know. As I already said in one of my previous points, languages are tools. It’s what you make with them which matters. Good engineer knows which tools are suitable for certain jobs and which aren’t.

Stop learning languages and start writing apps.

&nbsp;

## What OS should I use for developing apps, Windows, Linux or macOS?

It doesn’t really matter, except if you develop platform-specific apps. If you want to develop apps for platform X, it’s the best choice to actually do it on platform X. For iOS, it’s best to develop apps for it on macOS as primary development tool, XCode, is made for it. If platform X is Android, it doesn’t matter because ADK is available for all of these platforms (apply the same pattern for all other platforms). In other cases, it doesn’t really matter.

If you're a web developer, knowing Linux will be quite beneficial, as most web servers today use it. Don't worry if you never used it before. It's possible to play with Linux without actually installing it on the hardware (feature known as Live CD). The only thing you need is a USB stick and an ISO image of the distribution you want to try (Ubuntu will be easiest).

See also: [Garry Taylor's answer to Is Windows really a bad programming OS?](https://www.quora.com/Is-Windows-really-a-bad-programming-OS/answer/Garry-Taylor-5)

&nbsp;

## How much hours a day do I need to spend to learn programming?

It depends on your schedule (if you have any). There’s no definite answer for this - you should find some balance, but as it’s true with almost anything else, the more time you spend learning and practicing, the more things you will learn.

However, don’t forget that brain needs the rest too. The best way to learn new things is to practice them so, once you're finished with the basics, try to make some small projects in that language.

&nbsp;

## Why company X doesn’t rewrite it’s application in language Y but stays with language Z?

Because rewriting whole applications in other language is:

* costly
* time consuming
* introduces possibility of new bugs

Rewriting application in another language is almost always a wrong choice. Check [this article](https://www.joelonsoftware.com/2000/04/06/things-you-should-never-do-part-i/) by Joel Spolsky.

&nbsp;

## Linus Torvalds said that C++ is crap, it must be true because he wrote such a big project as Linux, right?

Nope. Linus is human being and human beings, because of their imperfect nature, have their prejudices.

Check out this article: [A response to Linus Torvalds on C++](http://warp.povusers.org/OpenLetters/ResponseToTorvalds.html).

&nbsp;

## How much time does it take to learn a programming language X?

It depends on your experience. If programming language X is, for example, Haskell, which is pure functional language, and you had prior experience only with imperative languages only, you’ll have to learn completely new paradigm, not just the new language.

Learning programming languages is easy, it’s writing software which is actually hard and needs much more effort.

&nbsp;

## Do I need to have a degree to get a job as a programmer?

For some jobs, having degree in computer science will be necessary.

If you have a change to get a degree, go for it. If you don't, focus more on building your portfolio.

&nbsp;

## What topics should I study to become a great programmer?

To become a great programmer, you should have good analytical skills and know how to find the best possible way to accomplish the goal in a limited time period. There is no definite answer to this question but in my opinion, you should become familiar with this topics:

* **Algorithms** - you don’t have to memorize them all, but you should get the idea when particular algorithm is the most suitable for your problem. You’ll learn how to implement basic once as you progress in your career.
* **Data Structures** - you should learn their memory and time complexity and again, when particular data structure is suitable for storing certain information.
* **Object-oriented Programming** - it’s the most important paradigm today and knowing it is mandatory.
* **Object-oriented Design Patterns** - if you want to further advance your knowledge of OOP (as you should), design patterns are the next station for you. Again, you don’t have to memorize them but to know when certain design pattern is a good choice for organizing part of the code base.
* **Software Documentation** - documenting your code helps you and your colleagues to better understand code base. By writing it, you’ll be thankful to yourself after few months / years as you’ll be much more able to understand what you wrote than if you left just code, without explaining what it does.
* **Testing** - it’s often told that project which has no tests is not complete (and I mostly agree with this). By writing unit tests, you ensure that your code base is stable and ready to be extended without disastrous consequences.

You should also learn how to write clean and organized code. For this topic, I recommend a book [Clean Code](https://www.amazon.com/Clean-Code-Handbook-Software-Craftsmanship/dp/0132350882) by Robert Cecil Martin.
