---
layout: post
title: Automating the AP Computer Science Exam
---
![Handwritten code](/assets/handwriting.png)

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
testing kind of a 'thing' in computer science?  

Automating the APCS Exam Part 1: Ingredients
--------------------------------------------

This is what I'm thinking about now, but now is also when I'm learning Ruby.
So I'm doing this with Ruby.  Instead of Java.

![Don't tell me what to do](/assets/dtmwtd.gif) 

Maybe I'll work up a Java version later.  The task will be to convert a 
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

The problem is a straightforward code kata. I feel comfortable not 
overthinking this.  The current scoring guide is geared toward functionality,
so I propose we write tests for a variety of cases, from the
trivial to the edge, and assign a point for each one.  In this way we will
have a score that measures the amount of desired functionality present in the
solution.  

Normalize the results for five points in the following way

  - 1 point for having the tests run (No points if the tests don't run: it's 
    a non-answer)
  - let n = the number of tests
  - let p = the number of passed tests
  - p/n * 4 points for functionality

The tests we'll need:

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
she pays attention to the test output, it's going to be immediately apparent that the 
problem is in dealing with multiple switches.  Now she can get to work finding 
a way to access the entire string. 

In Conclusion...
----------------

The expediency of automating the exam is obvious, but I'm not going to tell the
College Board how to spend their money. Actually, given that exam fees now
run $91, maybe someone should.  But not me, today.  Automating the exam is the
right thing to do *from a pedagogical point of view*. If I'm teaching to this 
test, I'm teaching  the current content plus careful code reading, analytical 
problem solving, and TDD. If I'm teaching to the old test, I'm teaching
penmanship.
