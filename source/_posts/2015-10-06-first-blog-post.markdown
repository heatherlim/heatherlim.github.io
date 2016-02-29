---
layout: post
title: "Why I Had Problems With Arrays/Hashes/Pigeons... (and what else I learned)"
date: 2015-10-06 21:04:50 -0400
comments: true
categories: 
- Flatiron School
---
Iteration was my first challenge that I didn't really know what to make of. I went through great pains to not use it. On the first lab where we had to write a method that could tel if a number was prime or not, instead of iterating through the whole array range to see if there were more than two zeroes (the array represented the number being divided by 1 to that particular number. So the first element in the array would be that number divided by one, the second element would be that number divided by two and so on... if the number was prime, the modulo should return only two 0s), I would instead sort the array and check if array[2] == 0. 

So already by avoiding it, I made it harder for myself to pick up. It was probably the first method that didn't look to be in plain English for me. It was pretty abstract.

```
basket = ["apple 1","apple 2","apple 3","apple 4","apple 5","apple 6","apple 7","apple 8","apple 9","apple 10"]

basket.each do |apple|
    puts "Taking out #{apple}"
end
```
It didn't click until later that the word between the | | could be anything. In this case it is apple because it describes each element of the array that is picked up. But it is only for ease of understanding. It could be |oranges| for all the program cares. 

It also helped me to visualize what's going on. Basically it was as though a pedantic person was going over each item in the array one by one and then executing whatever the instructions below told him to. So apple (or whatever is between the ||) would be equal to the string "apple 1" and then "apple 2" and so on...

It's a step by step process that I guess I could read as for each item in the array, do this to that item. And that started to make it clearer for me rather than what I thinking before. That it was some kind of magic thing that did something all at once. Figuring out the plain English equivalent made it also easier for me to finish the pigeon hash lab where we had to sort out pigeons by their names and attributes. The tricky part was understanding what exactly you were getting between those ||. 

Because I found iteration difficult, on my own time, I decided to make a cute program that would be able to take in a bunch of people and divide them into however many groups the user would want. The program would return different groups each time it was run.

For example, while the input would look something like this:

```
people = ["Adrian", "Polly", "John", "Ivan", "Jack", "Tom", "Anna", "Mary", "Ben", "Jeff",
"Alex", "Megan", "Helen", "Sarah", "Wei", "Aristo", "Jane", "Meredith", "Liz", "Lacey"]
```

The output will be in this format:
```
groups = {
   :group1 => [-names here-],
   :group2 => [-names here-],
   :group3 => [-names here-],
   :group4 => [-names here-],
   :group5 => [-names here-]
    }

```
Because I wanted to find a random person from the people array, I thought why not find a random number with rand() and make the number inside rand to be people.count? Then, if I named that variable something, I could call it into people[random] and it should return a random person from the array.

```
random = rand(people.count)
  people[random]

```
After this, I would be able to shovel in random people into their arrays as long as the people inside each of their arrays would not exceed the people.length/groupnumber. Which makes sense because if the array has 20 people in it, and we want to make five groups, each group would have four people in it.

However, even though I made progress with feeling more comfortable with iteration, I found it difficult to return a different random person each time so my groups would always be filled with the same random person. My logic was something like how can I iterate over my random variable if it wasn't an array? Then I tried some stuff with iterating over the people array and only shoveling in each person if the person value was equal to the random person value...

In any case, it didn't quite work out and I found a different method to get four different people into the array. So first I wanted to make an array with variables in it equal to the number of people I wanted in the group. So let that variable be called people_in_group (which would be the length of the people array divided by the number of groups we want).

```
Array.new(people_in_group)

```
Now this should create an array with a bunch of nils. But we can use .sample

.sample can choose a random element from the array

```
Array.new(people_in_group){people.sample}

```
This will fill the new array with random people until the number of elements it has is equal to the number of people we want in the group.

Right now I'm working on making sure there are no repeated people inside each array. Then I will make sure the people do not repeat in each group.

So hopefully completing this mini program will help me out further with iteration and sorting. In any case, maybe I'll be qualified enough as an entry-level pigeon sorter.

My current trouble is now with classes. So hopefully by the next time I make another post I'll be confident enough to write a little about it.