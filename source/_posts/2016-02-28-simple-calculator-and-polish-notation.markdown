---
layout: post
title: "Simple Calculator, Order of Operations, and Polish Notation"
date: 2016-02-28 13:18:40 -0500
comments: true
categories: 
- Polish Notation
- Prefix Notation
- Order of Operations
- Stack
---
One of the very first exercises I did as a coding exercise was creating a simple (and crappy) calculator. It was very basic and did not need to consider order of operations. Below is a really basic example of what I mean:
<script> 
$(function(){
  var calculator = new Calculator();// write your solution here.
});
// classes are dots and ids are #
function Calculator(){
  // This is a listener that listens for everryyy button click
  $('button').click(function(){
    // Now when a button is clicked, we are in a scope deeper than window
    // So when we call "this" we will get whatever button was clicked
    // Instead of getting the window
    if($(this).siblings('#result').length){
      var number1 = $('#number1 > .number').text()
      var number2 = $('#number2 > .number').text()
      var operation = $('#operation').text()
      var answer = eval([number1,operation,number2].join(''))
      $('#result').text(answer)   
    }else if(!$(this).siblings('#operation').length){    
      var oldValue = $(this).siblings('h2').text()
      var newValue = eval(oldValue + $(this).text() + 1)
      $(this).siblings('h2').text(newValue)
    } else if ($(this).siblings('#operation').length > 0) {
      $(this).siblings('h2').text($(this).text())      
    }
  })
}
</script>


<table border = 1 width = 600>
<tr>
  <td>
    <div id="number1">
    <h2 class="number">0</h2>
    <button class="incr" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">+</button>
    <button class="decr" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">-</button>
    </div>
  </td>
  <td>
    <div>
    <h2 id="operation">+</h2>
    <button id="add" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">+</button>
    <button id="sub" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">-</button>
    <button id="mult" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">*</button>
    <button id="div" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">/</button>
  </div>
  </td>
  <td>
    <div id="number2">
    <h2 class="number">0</h2>
    <button class="incr" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">+</button>
    <button class="decr" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">-</button>
    </div>
  </td>
  <td>
     <h2 id="result">0</h2>
      <button id="equals" style ="background-color: white; border: none; height: 17px; width: 17px; font-size: 16px; text-align: center;">=</button>
  </td>
</tr>
</table>

But what if you wanted to calculate using more than one operation? Or be even more flexible and let a user pass in a string to calculate? In this situation, order of operations becomes something to take note of. When researching this, I stumbled upon polish notation (also called prefix notation) and the use of stacks as a solution to this problem.

Polish notation is a different way to write arithmatic. The operator is placed to the left of the operand. This was initially strange to me because I was used to infix notation. In infix, we place the operator in between the operands we are calculating. I did not realize there was any other way and infix was the most intuitive to me. It's the more conventional way we read expressions.

An example of infix:<br>
2 + 3

An example of prefix:<br>
\+ 2 3


While infix might be easier for me to read, I found that when trying to make a calculator, prefix was easier for a computer. One reason is that the rules of order of operations becomes unnecessary with prefix. When trying to calculate infix, there are a lot of rules to keep in mind. 

For example:<br>
(5 - 6) * 7

We need to evaluate what is within the parenthesis first. Also, if there were no parenthesis, we would have to calculate 6 * 7 first as multiplication comes before subtraction. Parenthesis are very necessary in infix as they can change what an expression evaluates to with removal.

The same expression however, when written in prefix does not need parenthesis:<br>
\* - 5 6 7

The order in which a prefix is calculated is always going to be conveyed by how it is arranged. All prefix expressions are calculated in the same way. To further elaborate:

With infix, finding the innermost expression to calculate first is difficult. It requires a lot of jumping around in the expression. <br>
((15 / (7 - (1 + 1))) * 3) - (2 + (1 + 1))

But if it was written in prefix:<br>
\- * / 15 - 7 + 1 1 3 + 2 + 1 1

This prefix expression is calculated in the same way that our previous simple example (+ 2 3) is calculated. We do not even need to memorize the order of operations. Example below on what I mean.

Calculating Prefix Step by Step:<br>
- Read from right to left<br>
- Look for two operands and their operator and evaluate them

\- * / 15 - 7 + 1 1 3 + 2 <strong>+ 1 1</strong>

Now we can evaluate them:<br>
\- * / 15 - 7 + 1 1 3 + 2 <strong>2</strong>

Repeat:<br>
\- * / 15 - 7 + 1 1 3 4

Our next two operands and operators are selected again. From right to left, the two operands next to their operator will be:<br>
\- * / 15 - 7 <strong>+ 1 1</strong> 3 4<br>
<em>(As you can see, from this example, just calculating prefix in this way does not need a knowledge of order of operations... there's no jumping around in an expression. You just have to follow <strong> one </strong> rule)</em>

This is what we have now:<br>
\- * / 15 <strong>- 7 2</strong> 3 4<br>
We can see the next two to caclulate is - 7 2... and so on...<br>
\- * <strong>/ 15 5 </strong>3 4 <br> 
\- <strong>* 3 3</strong> 4 <br>
\- 9 4 <br>
And now...<br>
The answer is <strong> 5 </strong>

I wrote a program in Ruby that calculates prefix. Here's a snippet of a part of its code (the other methods I didn't include because I felt they were self-explanatory):
```
def calculate_prefix
  stack = []
  # @arg is the prefix expression
  loopSize = @arg.count
    for i in 0..loopSize
    value = @arg.pop
      if operand?(value)
        stack << value
      elsif operator?(value)
        operator1 = stack.pop
        operator2 = stack.pop
        stack << compute(value, operator1, operator2)
      end
    end
    stack.compact.first
  end
```
Notice I am making an array called stack. In Ruby, an array can pretty much function as a stack. In other languages, there is an actual stack datatype. Stack is a last-in-first-out data structure. You can make a Ruby implementation of a stack which RubyMonk demonstrates <a href = "https://rubymonk.com/learning/books/4-ruby-primer-ascent/chapters/33-advanced-arrays/lessons/86-stacks-and-queues">HERE</a>.

Basically the last thing I push into a stack will be the first thing I can pull out. So if using a Ruby array, there are just two array methods I would use to simulate a stack. As demonstrated below.

```
# adding to a stack
  stack << something
# retrieving from a stack
  stack.pop
```
You can tell I used both of those in my calculating prefix method.

So an explanation of my method:
I'm reading the prefix expression from right to left, popping a value from it. If the value is an operand, I will be pushing it onto my stack. Otherwise, if it is an operator, I'll pop the first two elements in my stack and compute them before pushing them to the stack again. 

You can tell this is a lot easier than if I were to calculate infix directly. 

Prefix might not be the easiest way for me to read an expression but it makes it easier for me to deal with coding a simple calculator. It's convenience over convention.

Reference:<br>
<a href="https://en.wikipedia.org/wiki/Polish_notation">https://en.wikipedia.org/wiki/Polish_notation</a>