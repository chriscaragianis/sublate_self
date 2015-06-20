---
layout: post
title: Automating the Ap Computer Science Exam
---
The silliest thing about AP Computer Science is the handwriting of code. 
Students give you side eye when you bring it up and all you can do is shrug. 
That’s [the test](https://apstudent.collegeboard.org/apcourse/ap-computer
-science-a), and it doesn’t matter what I think, nor what is right, true or 
sensible.

It doesn’t have to be this way
------------------------------

Why not have a computer based test
like the GRE or the Praxis?  Give the kids some syntax highlighting for crying 
out loud!  Better yet, why not *automate* the test?  Grading the test takes a 
fleet of highly qualified graders a week.  Expenses paid. Isn't automated 
testing kind of a 'thing' in software development today?  In fact, when we 
look at the test, we're going to see that it lends it self quite well to 
automated testing.

Automating the APCS Exam Part 1: Ingredients
--------------------------------------------

We’ll start with the happy assumption that the College Board [replaces Java 
with Ruby](LINK TO OTHER POST) as the language of study. As usual, the most
important reason (among many nearly as important and far more valid reasons)
for this is that I want to work with Ruby.  The task will be to convert last 
year’s handwritten free-response section to an automated, computer-based test.

We'll write the tests using [RSpec](rspec.info), again, because that's what I
feel like messing around with right now. Incidentally, I had intended to use
acceptance testing as well, but the way the APCS exam is written and graded, 
unit tests suffice.  This might be indicative of a need for better test items.


Automating the APCS Exam Part 2: The Directions 
------------------------------------------------------

Lets start with this: 

![APCS Free Response Directions](/assets/directions.png) 
First off we have to assume that *SHOW ALL WORK* was left over from the 
Calculus exam some one was cribbing the format from.  Then of course we can 
ignore the bit about Java. Rather than use a "quick reference", lets allow
students a local copy of [ruby-doc.org](ruby-doc.org). 

The second bullet is where we can begin to make some meaningful changes. 
Assumptions don't have to be made, conventions can be *enforced* by our tests.
Indeed, this ought to incentivize the test taker to read the tests. This is a 
good thing.

Finally, we have the third bullet, which can be replaced by a common-sense
limit on the number of lines of code a student can submit.  Something long 
enough to allow clarity and unexpected approached to the problem, but succinct
enough to prevent any wheel reinventions.  

Question 1
------------------------------------------------------

Here are all three parts of Question 1:

![Question 1 part a](/assets/q1a.png) 
![Question 1 part b](/assets/q1b.png) 
![Question 1 part c](/assets/q1c.png) 


