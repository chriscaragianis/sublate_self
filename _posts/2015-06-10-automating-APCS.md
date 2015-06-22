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
testing kind of a 'thing' in software development today?  

Automating the APCS Exam Part 1: Ingredients
--------------------------------------------

We’ll start with the happy assumption that the College Board [replaces Java 
with Ruby](LINK TO OTHER POST) as the language of study. As usual, the most
important reason (among many nearly as important and far more valid reasons)
for this is that I want to work with Ruby.  The task will be to convert a 
handwritten free-response question from 2014 into an automated, computer-based 
test with automated scoring.

We'll write the tests using [RSpec](rspec.info), again, because that's what I
feel like messing around with right now. 

Here are the directions for part (a) of Question 1 from 
2014's exam:

![Question 1](/assets/directions_a.png)

And the scoring guide:

![Question 2](/assets/score_guide.png)

Automating the APCS Exam Part 2: Designing the tests
-----------------------------------------------------

We're going to assign five points on this problem, but we are going to do it 
differently.

*I'm going to get teacher-y here, bail on this section if you must.*

Good teachers assess diverse aspects of performance. It's good practice to
value all parts of creating an artifact, so that students with diverse 
skills can see success in different ways. So, penmanship counts.  

On the other hand, this can get out of hand.  Especially if you have a 
target number of points, or have a task that is difficult to parse into 
discrete "jobs".  What we have with this scoring guide is a bit of *partial
credit creep*. 

The problem is a straightforward code kata. The scoring guide seems like it's 
trying to extract more computer science knowledge than the problem can
really support.  I propose we write tests for a variety of cases, from the
trivial to the edge, and assign a point for each one.  In this way we will
have a score that measures the amount of desired functionality present in the
solution.  We'll normalize the results for five points in the following way

  - 1 point for having the tests run (No points if the tests don't run: it's 
    a non-answer)
  - let n = the number of tests
  - let p = the number of passed tests
  - p/n * 4 points for functionality

Now we can get our finer grained assessment by writing tests that cover the 
various ways the solution can fail in a modular fashion.

  - No change if the pattern does not appear (include a trailing 'A')
  - One change, beginning of string
  - One change, middle of string
  - One change, end of string
  - No change on 'AA'
  - Multiple changes, no chance of double changes
  - Multiple changes, avoid double changes

So our tests will look like this:

~~~ ruby
  describe 'scramble_word: No change if no A_' do
    let (:word) {'MOTHRA'}
    it { should eq 'MOTHRA' }
  end

  describe 'scramble_word: One change, at beginning' do
    let (:word) {'APPLE'}
    it { should eq 'PAPLE' }   
  end

  describe 'scramble_word: One change, in middle' do
    let (:word) {'TABLE'}
    it { should eq 'TBALE' }   
  end

  describe 'scramble_word: One change, at end' do
    let (:word) {'LINEAR'}
    it { should eq 'LINERA' }   
  end

  describe 'scramble_word: No switch and skip on AA' do
    let (:word) {'AARP'}
    it { should eq 'ARAP' }   
  end  
  
  describe 'scramble_word: Multiple switches, no double switch' do
    let (:word) {'ANNATO'}
    it { should eq 'NANTAO' }   
  end  
 
  describe 'scramble_word: Multiple switches, handle double switch' do
    let (:word) {'BANANA'}
    it { should eq 'ABNNAA' }   
  end  
~~~

I should note that these are what the *exam-taker's* tests will look like. For 
obvious reasons, we should have a separate set of analogous examples for 
scoring.  

Automating the APCS Exam Part 3: Scoring a Response
-----------------------------------------------------

So let's say we have a student sitting for the exam, she encounters this 
problem, and works out this solution.

~~~ ruby
class Scramble

  def self.scramble_word word
    word.sub(/A./) { |m| m.reverse }
  end
end
~~~

If she stops here, then four of the seven tests will pass, yielding a score of
3.29/5 for the problem, which is comparable to the 3/5 she would have scored
under the old scoring guide.   

The real pedagocial benefit to this approach is that she *wouldn't* stop here.
It slipped her mind that <code>sub</code> only looks at the first match.  If
she looks at the test output, it's going to be immediately apparent that the 
problem is in dealing with multiple switches.  Now she can get to work finding 
a way to work on the entire string.  
