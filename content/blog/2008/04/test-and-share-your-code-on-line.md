---
title: "Test and Share Your Code On-Line"
date: "2008-04-18"
tags: [programming,tools,webdev]
---

[Codepad](http://codepad.org/) is a pastebin that runs your code for you. It was written by [Steven Hazel](http://www.hackerdashery.com/). Codepad works like a compiler or interpreter, not like the read-eval-print loop of an interactive interpreter prompt. If you want to print the value of an expression, you need to use your language's print command. If your language requires a "main" function, your program won't work in codepad without one.

Steven has added support for many languages, including PHP, Python, Ruby and Perl.

**How it works**

Code execution is handled by a supervisor based on [geordi](http://www.xs4all.nl/%7Eweegen/eelis/geordi/). The strategy is to run everything under ptrace, with many system calls disallowed or ignored. Compilers and final executables are both executed in a chroot jail, with strict resource limits. The supervisor is written in Haskell.

"Hello World" examples: [http://codepad.org/hello-world](http://codepad.org/hello-world)

Excellent tool!
