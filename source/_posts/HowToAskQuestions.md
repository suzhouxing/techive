---
title: 如何咨询技术类问题
date: 2017-10-02 14:14:37
categories:
- 技术
tags:
- 技术
- 提问
---
每当有人发来一张出错提示框的截图来问我怎么办的时候, 我的内心都是崩溃的.

对于这种让人摸不着头脑的提问方式, 我真想说: "你把这个问题发到 StackOverflow 上, 两天内没被维护者关闭的话把链接发给我".

在这里汇总一下技术类提问指南, 免得不会问问题的提问者又来问我为什么我在 StackOverflow 上发的问题被关闭了.

[1] https://stackoverflow.com/help/how-to-ask
[2] https://stackoverflow.com/help/mcve
[3] https://ericlippert.com/2014/03/05/how-to-debug-small-programs/



> # How do I ask a good question?
> 
> We'd love to help you. To **improve your chances** of getting an answer, here are some tips:
> 
> ## [Search](https://stackoverflow.com/search), and research
> 
> ...and keep track of what you find. Even if you don't find a useful answer elsewhere on the site, including links to related questions that *haven't* helped can help others in understanding how your question is different from the rest.
> 
> ## Write a title that summarizes the specific problem
> 
> The title is the first thing potential answerers will see, and if your title isn't interesting, they won't read the rest. So *make it count:*
> 
> - **Pretend you're talking to a busy colleague** and have to sum up your entire question in one sentence: what details can you include that will help someone identify and solve your problem? Include any error messages, key APIs, or unusual circumstances that make your question different from similar questions already on the site.
> - **Spelling, grammar and punctuation are important!** Remember, this is the first part of your question others will see - you want to make a good impression. If you're not comfortable writing in English, ask a friend to proof-read it for you.
> - If you're having trouble summarizing the problem, **write the title last** - sometimes writing the rest of the question first can make it easier to describe the problem.
> 
> Examples:
> 
> - **Bad:** C# Math Confusion
> - **Good:** Why does using float instead of int give me different results when all of my inputs are integers?
> - **Bad:** [php] session doubt
> - **Good:** How can I redirect users to different pages based on session data in PHP?
> - **Bad:** android if else problems
> - **Good:** Why does str == "value" evaluate to false when str is set to "value"?
> 
> ## Introduce the problem before you post any code
> 
> In the body of your question, start by expanding on the summary you put in the title. Explain how you encountered the problem you're trying to solve, and any difficulties that have prevented you from solving it yourself. The first paragraph in your question is the second thing most readers will see, so make it as engaging and informative as possible.
> 
> ## Help others reproduce the problem
> 
> Not all questions benefit from including code. But if your problem is *with* code you've written, you should include some. But **don't just copy in your entire program!** Not only is this likely to get you in trouble if you're posting your employer's code, it likely includes a lot of irrelevant details that readers will need to ignore when trying to reproduce the problem. Here are some guidelines:
> 
> - Include just enough code to allow others to reproduce the problem. For help with this, read [How to create a Minimal, Complete, and Verifiable example](https://stackoverflow.com/help/mcve).
> - If it is possible to create a live example of the problem that you can *link* to (for example, on <http://sqlfiddle.com/> or <http://jsbin.com/>) then do so - but also include the code in your question itself. Not everyone can access external sites, and the links may break over time.
> 
> ## Include all relevant tags
> 
> Try to include a tag for the language, library, and specific API your question relates to. If you start typing in the tags field, the system will suggest tags that match what you've typed - be sure and read the descriptions given for them to make sure they're relevant to the question you're asking! See also: [What are tags, and how should I use them?](https://stackoverflow.com/help/tagging)
> 
> ## Proof-read before posting!
> 
> Now that you're ready to ask your question, take a deep breath and read through it from start to finish. Pretend you're seeing it for the first time: *does it make sense?* Try reproducing the problem yourself, in a fresh environment and make sure you can do so using only the information included in your question. Add any details you missed and read through it again. Now is a good time to make sure that your title still describes the problem!
> 
> ## Post the question and respond to feedback
> 
> After you post, leave the question open in your browser for a bit, and see if anyone comments. If you missed an obvious piece of information, be ready to respond by editing your question to include it. If someone posts an answer, be ready to try it out and provide feedback!
> 
> ## Look for help asking for help
> 
> In spite of all your efforts, you may find your questions poorly-received. Don't despair! Learning to ask a good question is a worthy pursuit, and not one you'll master overnight. Here are some additional resources that you may find useful:
> 
> - [Writing the perfect question](http://codeblog.jonskeet.uk/2010/08/29/writing-the-perfect-question/)
> - [How do I ask and answer homework questions?](http://meta.stackexchange.com/questions/10811/how-do-i-ask-and-answer-homework-questions)
> - [How to debug small programs](http://ericlippert.com/2014/03/05/how-to-debug-small-programs/)
> - [Meta discussions on asking questions](http://meta.stackexchange.com/questions/tagged/asking-questions)
> - [How to ask questions the smart way](http://www.catb.org/~esr/faqs/smart-questions.html) — long but good advice.



> # How to create a Minimal, Complete, and Verifiable example
>
> When asking a question about a problem caused by your code, you will get much better answers if you provide code people can use to reproduce the problem. That code should be…
>
> - …Minimal – Use as little code as possible that still produces the same problem
> - …Complete – Provide all parts needed to reproduce the problem
> - …Verifiable – Test the code you're about to provide to make sure it reproduces the problem
>
> ## Minimal
>
> The more code there is to go through, the less likely people can find your problem. Streamline your example in one of two ways:
>
> 1. **Restart from scratch.** Create a new program, adding in only what is needed to see the problem. This can be faster for vast systems where you think you already know the source of the problem. Also useful if you can't post the original code publicly for legal or ethical reasons.
> 2. **Divide and conquer.** When you have a small amount of code, but the source of the problem is entirely unclear, start removing code a bit at a time until the problem disappears – then add the last part back.
>
> ### Minimal *and* readable
>
> Minimal does not mean *terse* – don't sacrifice communication to brevity. Use consistent naming and indentation, and include comments if needed to explain portions of the code. Most code editors have a shortcut for formatting code – find it, and *use it!*Also, **don't use tabs** – they may look good in your editor, but they'll just make a mess on Stack Overflow.
>
> ## Complete
>
> Make sure all information necessary to reproduce the problem is included:
>
> - Some people might be prepared to load the parts up, and actually try them to test the answer they're about to post.
> - The problem might not be in the part you suspect it is, but another part entirely.
>
> If the problem requires some server-side code as well as an XML-based configuration file, include them both. If a web page problem requires HTML, some JavaScript and a stylesheet, include all three.
>
> ## Verifiable
>
> To help you solve your problem, others will need to verify that it *exists:*
>
> - **Describe the problem.** "It doesn't work" is not a problem statement. Tell us what the expected behavior should be. Tell us what the exact wording of the error message is, and which line of code is producing it. Put a brief summary of the problem in the title of your question.
> - **Eliminate any issues that aren't relevant to the problem.** If your question isn't *about* a compiler error, ensure that there are no compile-time errors. Use a program such as [JSLint](http://www.jslint.com/) to validate interpreted languages. [Validate](http://validator.w3.org/) any HTML or XML.
> - **Ensure that the example actually reproduces the problem!** If you inadvertently fixed the problem while composing the example but didn't test it again, you'd want to know that before asking someone else to help.
>
> It might help to shut the system down and restart it, or transport the example to a fresh machine to confirm it really does provide an example of the problem.
>
> For more information on how to debug your program so you can create a minimal example, [Eric Lippert](http://stackoverflow.com/users/88656/eric-lippert) has a fantastic blog post on the subject: *How to debug small programs*.
>
> You may have been told to include an MCVE by some helpful commentary, or perhaps even an MVCE if they were rushed; sorry for the initialisms, this is what they were referring to.



> # How to debug small programs
>
> Posted on [March 5, 2014](https://ericlippert.com/2014/03/05/how-to-debug-small-programs/)
>
> One of the most frequent categories of bad questions I see on StackOverflow is:
>
> > *I wrote this program for my assignment and it doesn't work.*
> > *[20 lines of code].*
>
> And... that's it.
>
> If you're reading this, odds are good it's because I or someone else linked here from your StackOverflow question shortly before it was closed and deleted. (If you're reading this and you're not in that position, consider leaving your favourite tips for debugging small programs in the comments.)
>
> StackOverflow is a question-and-answer site for specific questions about actual code; "*I wrote some buggy code that I can't fix*" is not a *question*, it's a *story*, and not even an interesting story. "*Why does subtracting one from zero produce a number that is larger than zero, causing my comparison against zero on line 12 to incorrectly become true?*" is a *specific question about actual code*.
>
> So you're asking the Internet to *debug a broken program that you wrote*. You've probably never been taught how to debug a small program, because let me tell you, **what you're doing now is not an efficient way to get that problem solved**. Today is a good day to learn how to debug things for yourself, because StackOverflow is not about to debug your programs for you.
>
> I'm going to assume that your program actually compiles but its action is wrong, and that moreover, you have a test case that shows that it is wrong. Here's how to find the bug.
>
> First, **turn on all compiler warnings**. There is no reason why a 20 line program should produce even a single warning. Warnings are the compiler telling you "this program compiles but does not do what you think it does", and *since that is precisely the situation you are in*, it behooves you to pay attention to those warnings.
>
> **Read them very carefully**. If you don't understand why a warning is being produced, that's a good question for StackOverflow because it is a *specific question about actual code*. Be sure to post the exact text of the warning, the exact code that produces it, and the exact version of the compiler you're using.
>
> If your program still has a bug, **obtain a rubber duck**. Or if a rubber duck is unavailable, get another computer science undergraduate, it's much the same. Explain to the duck using simple words why each line of each method in your program is obviously correct. At some point you will be unable to do so, either because you don't understand the method you wrote, or because it's wrong, or both. Concentrate your efforts on that method; *that's probably where the bug is*. Seriously, rubber duck debugging works. And as legendary programmer Raymond Chen points out in a comment below, if you can't explain to the duck why you're executing a particular statement, maybe that's because you started programming before you had a plan of attack.
>
> Once your program compiles cleanly and the duck doesn't raise any major objections, if there's still a bug then see if you can **break your code up into smaller methods**, each of which does **exactly one logical operation**. A common error amongst all programmers, not just beginners, is to make methods that try to do multiple things and do them poorly. Smaller methods are easier to understand and therefore easier for both you and the duck to see the bugs.
>
> While you're refactoring your methods into smaller methods, take a minute to **write a technical specification for each method**. Even if it is just a sentence or two, having a specification helps. The technical specification describes what the method does, what legal inputs are, what expected outputs are, what error cases are, and so on. Often by writing a specification you'll realize that you forgot to handle a particular case in a method, and that's the bug.
>
> If you've still got a bug then first double check that your specifications contain all the **preconditions and postconditions **of every method. A precondition is a thing that has to be true before a method body can work correctly. A postcondition is a thing that has to be true when a method has completed its work. For example, a precondition might be "this argument is a valid non-null pointer" or "the linked list passed in has at least two nodes", or "this argument is a positive integer", or whatever. A postcondition might be "the linked list has exactly one fewer item in it than it had on entry", or "a certain portion of the array is now sorted", or whatever. A method that has a precondition violated indicates a bug in the caller. A method that has a postcondition violated even when all its preconditions are met indicates a bug in the method. Often by stating your preconditions and postconditions, again, you'll notice a case that you forgot in the method.
>
> If you've still got a bug then learn how to write **assertions** that verify your preconditions and postconditions. An assertion is like a comment that tells you when a condition is violated; a violated condition is almost always a bug. In C# you can say `using System.Diagnostics;` at the top of your program and then `Debug.Assert(value != null);`or whatever. Every language has a mechanism for assertions; get someone to teach you how to use them in your language. Put the precondition assertions at the top of the method body and the postconditions before the method returns. (Note that this is easiest to do if every method has a single point of return.) Now when you run your program, if an assertion fires you will be alerted to the nature of the problem, and it won't be so hard to debug.
>
> Now **write test cases **for each method that verify that it is behaving correctly. Test each part independently until you have confidence in it. Test a lot of simple cases; if your method sorts lists, try the empty list, a list with one item, two items, three items that are all the same, three items that are in backwards order, and a few long lists. Odds are good that your bug will show up in a simple case, which makes it easier to analyze.
>
> Finally, if your program still has a bug, **write down on a piece of paper the exact action you expect the program to take on every line of the program for the broken case**. Your program is only twenty lines long. You should be able to write down *everything* that it does. Now step through the code **using a debugger**, examining every variable at every step of the way, and line for line verify what the program does against your list. If it does anything that's not on your list then either your list has a mistake, in which case you didn't understand what the program does, or your program has a mistake, in which case you coded it wrong. Fix the thing that is wrong. If you don't know how to fix it, at least now you have a specific technical question you can ask on StackOverflow! Either way, iterate on this process until the description of the proper execution of the program and the actual execution of the program match.
>
> While you are running the code in the debugger I encourage you to **listen to small doubts**. Most programmers have a natural bias to believe their program works as expected, but you are debugging it because that assumption is wrong! Very often I've been debugging a problem and seen out of the corner of my eye the little highlight show up in Visual Studio that means "a memory location was just modified", and I know that memory location has nothing to do with my problem. So then *why was it modified*? Don't ignore those nagging doubts; study the odd behaviour until you understand why it is either correct or incorrect.
>
> If this sounds like a lot of work, that's because it is. If you can't do these techniques on twenty line programs that you wrote yourself you are unlikely to be able to use them on two million line programs written by someone else, but that's the problem that developers in industry have to solve every day. Start practicing!
>
> And the next time you write an assignment, **write the specification, test cases, preconditions, postconditions and assertions for a method before you write the body of the method!** You are much less likely to have a bug, and if you do have a bug, you are much more likely to be able to find it quickly.
>
> This methodology will not find every bug in every program, but it is highly effective for the sort of short programs that beginner programmers are assigned as homework. These techniques then scale up to finding bugs in non-trivial programs.
