---
title: "Java, C, Python and nested loops"
date: "2009-06-16"
tags: [java,programming,python,software-architecture]
---

Java has no goto statement, to break or continue multiple-nested loop or switch constructs, Java programmers place labels on loop and switch constructs, and then break out of or continue to the block named by the label. The following example shows how to use java break statement to terminate the labeled loop:

```
public class BreakLabel
{
    public static void main(String[] args)
    {
        int[][] array = new int[][]{{1,2,3,4},{10,20,30,40}};
        boolean found = false;
        System.out.println("Searching 30 in two dimensional int array");

        Outer:
        for (int intOuter = 0; intOuter < array.length ; intOuter++) {
            Inner:
            for (int intInner = 0; intInner < array[intOuter].length; intInner++) {
                if (array[intOuter][intInner] == 30) {
                    found = true;
                    break Outer;
                }
            }
        }

        if (found == true) {
            System.out.println("30 found in the array");
        } else {
            System.out.println("30 not found in the array");
        }
    }
}
```

Use of labeled blocks in Java leads to considerable simplification in programming effort and a major reduction in maintenance.

On the other hand, the C continue statement can only continue the immediately enclosing block; to continue or exit outer blocks, programmers have traditionally either used auxiliary Boolean variables whose only purpose is to determine if the outer block is to be continued or exited; alternatively, programmers have misused the goto statement to exit out of nested blocks.

What's interesting is that Python [rejected](http://www.python.org/dev/peps/pep-3136/) the labeled break and continue proposal a while ago. And here's why:

> Guido van Rossum wrote:
> 
> I'm rejecting it on the basis that code so complicated to require this feature is very rare. While I'm sure there are some (rare) real cases where clarity of the code would suffer from a refactoring that makes it possible to use return, this is offset by two issues:
> 
> 1\. The complexity added to the language, permanently.
> 
> 2\. My expectation that the feature will be abused more than it will be used right, leading to a net decrease in code clarity (measured across all Python code written henceforth). Lazy programmers are everywhere, and before you know it you have an incredible mess on your hands of unintelligible code.

But what's more interesting is that the idea of adding a goto statement was never mentioned. Common sense perhaps?
