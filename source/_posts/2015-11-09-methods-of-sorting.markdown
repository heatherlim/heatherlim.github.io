---
layout: post
title: "Methods of Sorting and a Bit of Trailing Around from There"
date: 2015-11-09 22:14:16 -0500
comments: true
categories: 
- Flatiron School
---
While working on my sorting program, I came across several interesting methods that I didn't know before. While I haven't used any of them in my project thus far, I still found them pretty cool. From there I normally go on long google crawls until sometimes I learn things completely unrelated but still pretty cool. So this blog post is about my train of thought and how I get from sorting to something else entirely.

The first two methods I played around with were array methods called permutation and combination. 

An example of permutation:
```
[1, 2, 3].permutation.to_a
=> [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
[1, 2, 3].permutation(2).to_a
=> [[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]]
```
An example of combination:
```
[1, 2, 3].combination(2).to_a
=> [[1, 2], [1, 3], [2, 3]] 
```
Combination requires you to put in an argument while permutation does not. Permutation defaults to the number of elements in the array. Also notable is that there are always going to be more permutations than combinations because combination takes order of the elements into account so [1, 2] and [2, 1] are considered the same.

So how could I find all combinations in an array of people so that every person in the array would have met exactly once? So a group of Mary and Tom would be considered the same as a group of Tom and Mary. Well... there's a gem for that. It's called the round robin tournmanet gem. And I'll provide a link at the bottom. 

This is a portion of its  code that I copied from its github repository:
```
def self.schedule(array)
    array.push nil if array.size.odd?
    n = array.size
    pivot = array.pop
    games = (n - 1).times.map do
      array.rotate!
      [[array.first, pivot]] + (1...(n / 2)).map { |j| [array[j], array[n - 1 - j]] }
    end
    array.push pivot unless pivot.nil?
    games
  end

  # running it will give...

  self.schedule(a)
 => [[[2, 4], [3, 1]], [[3, 4], [1, 2]], [[1, 4], [2, 3]]] 
```
Pretty cool!

Interestingly enough however, while I find methods that work for sorting unique pairs, unique trios and so on... they all function very differently so I'm still looking for one that could work in all cases for my sorting project.

Another method I found helpful is a rails method called in_groups_of. It is used to split an array into groups of whatever number is put inside the argument. 

Here's an example of its usage:
```
%w(1 2 3 4 5 6 7 8 9 10).in_groups_of(3) {|group| p group}
["1", "2", "3"]
["4", "5", "6"]
["7", "8", "9"]
["10", nil, nil]
```
The example I saw was the first time I saw %w in use too. Putting the %w makes it so the elements are split up by the whitespace. So I would be able to get the same result if I had typed [1, 2, 3, 4, 5, 6, 7, 8, 9, 10].in_groups_of(3) as well. 

Note that it padded the last group with nil. There's a way around that if that isn't what you're looking for. For example:

```
%w(1 2 3 4 5).in_groups_of(2, false) {|group| p group}
["1", "2"]
["3", "4"]
["5"]
```

Here you can see there is a second argument placed. When false is written there, it will not pad the remaining spaces left over from the uneven grouping. Aside from false, you can also place anything else in there and the method will use that to pad the empty spaces. 

This method however is only available in rails. In ruby there is a similar method called each_slice. 

```
(1..10).each_slice(3) {|a| p a}
# outputs below
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
[10]
```
Aside from a range, it is also possible to use it on an array. Both of those methods work similar to the way my sorting program does. But it divides up an array based on how many elements one wants inside each new group rather than the group number. 

Something random I found interesting when I was experimenting was that when I called .class on the result of each_slice, I was getting a NilClass.

What I copied from my cmd line:
```
2.2.2 :010 > (1..10).each_slice(3) {|a| p a}
[1, 2, 3]
[4, 5, 6]
[7, 8, 9]
[10]
 => nil 
2.2.2 :011 > _.class
=> NilClass 
```
I thought it was interesting because I was using 'p' in the {|a| p a } part. A distinction between p and puts would be that p normally returns to me the actual class. Not nil. Maybe it returned nile because of something to do with the iteration? I'm not too sure.

Distinguising p and puts example:

```
2.2.2 :017 > p "1"
"1"
 => "1" 
2.2.2 :018 > _.class
 => String 
2.2.2 :019 > puts "1"
1
 => nil 
2.2.2 :020 > _.class
 => NilClass 
```
p calls on .inspect. Notie that the return value is not nil. So it was odd for me that in the each slice example it gave me a nil return value. I experimented a bit further with each slice to see what puts and print would give me if I placed it where the p was.

print:
```
2.2.2 :024 > (1..10).each_slice(3) {|a| print a}
[1, 2, 3][4, 5, 6][7, 8, 9][10] => nil 
```
Considering that print was like puts but does not add a new line, I was not too surprised to see this.

But when I used puts...
```
2.2.2 :025 > (1..10).each_slice(3) {|a| puts a}
1
2
3
4
5
6
7
8
9
10
 => nil 
```
It surprised me that it did not actually output the arrays but just the whole range. I'm not too sure why it did that. I assume on the first iteration a is [1, 2, 3] and so on... and used a pry to confirm it was too. So I wonder why it prints out the whole range instead of the array.

References:<br>
<a href = "http://chriscontinanza.com/2010/10/29/Array.html">Permutations and Combinations</a><br>
<a href = "http://apidock.com/rails/Array/in_groups_of">in groups of</a><br>
<a href = "http://apidock.com/ruby/Enumerable/each_slice">each slice</a><br>
<a href = "http://www.garethrees.co.uk/2013/05/04/p-vs-puts-vs-print-in-ruby/">difference between p, puts and print</a><br>
<a href = "https://github.com/ssaunier/round_robin_tournament">round robin gem</a>
