---
layout: post
title: "Things Coming Round Again and the Importance of Thinking Before Coding"
date: 2015-10-25 20:44:04 -0400
comments: true
categories: 
- Flatiron School
---
I'm still working on my cute random group sample code because spending hours solving a "problem" that probably takes up just ten minutes of your time doing manually is a fun thing to do. It really is. 

Anyway, it has made a lot of progress from before. I remembered that I was so uncomfortable with classes two weeks ago... and now... well I'm still not as comfortable as I'd like to be but I'm actually using them now which is a plus. Something I learned while working on this is that sometimes the best way to solve a problem is the simplest approach. I was so excited about using .sample that I ignored some of the more obvious methods to use. Instead, I used .sample and .pop to pull random people from the array.

This is a section of my original code:
```
def random_group(people, groupnumber)
  random_person = people.sample
  binding.pry
  group_array = [] 
   Array.new(groupnumber) {people.sample }
    people.each do |each_person|
      if each_person != random_person
        group_array << random_person
        people.delete_if {|repeat_person| repeat_person == random_person}
        binding.pry
      elsif each_person == random_person
        people.delete_if {|repeat_person| repeat_person == random_person}
      end     
    end
```
If you can't decipher what the hell is going on, that's fine because it didn't work and I could barely tell myself what I was doing. First warning sign I suppose that I should find a different approach. 

My problem I outlined in my previous post was that .sample would take a random person out of the array but there when the method was run again, there was still a chance to pick the same person out of the array. The person wasn't actually removed in the array. While I had thought I could use .pop, that method would not be random. 

The solution was to use .shuffle before .pop was used. This way, I can now pick random people out of the array until it was empty. I wasted a lot of time with .sample because I was too focused in using .sample than using the right tool for the job. I'm not saying using .sample is impossible but .shuffle and .pop made mroe sense to me. 

Now my code has two methods that make the hash and fill it.

```
def create_groups 
     range = (1..@groupnumber).to_a
     range.each do |indgroup| # 1, 2, 3, 4...
     @groups["group" + " " + "#{indgroup}"] = []
    end  
    @groups
  end

  def filling_group
    people_number = @people.count/@groupnumber
    range = (1..@groupnumber).to_a
     range.each do |indgroup|
        @groups["group" + " " + "#{indgroup}"] << @people.shuffle!.pop(people_number)    
          @groups.values.each do |x|
            x.flatten!
         end
     end 
  end
```
With this, I had fulfilled the first goal I set. Right now, my code could create x number of groups for x amount of people. I added the code below as well so that it would be possible for people to be divided into groups even if the groups were uneven. Like 20 people could be divided into 7 groups and so on.

```
def if_uneven
    range = (1..@people.count).to_a
    range.each do |indgroup|
      @groups["group" + " " + "#{indgroup}"] << @people.shuffle!.pop
    end
  end
```
After people are sorted into groups, the remaining number of people would in the people array would be iterated through again and distributed amoungst the groups already made. 

And now comes the part where thinking ahead would have really helped me out. I had thought that being able to sort people randomly into groups was the first step in my project. The second step would be to show all other unique possibilities of groups. Though I could keep running the program, it does not actually "remember" the last distribution it made. So it is highly possible that if Person 1 and Person 2 were paired together, they could be paired together again whether in the same group or a different one. 

I spent awhile thinking about how to make the program remember what groups each person was put into and make sure they'd never be in a group where some of their original group mates were. Should I use databases to store this information? Or I could make an even bigger hash. I could place 
```
groups = {
  :group1 => [],
  :group2 => [],
  :group3 => [],
  :group4 => [],
  :group5 => []
}
```
into a hash with values of days. So it would be Day 1 => groups... Day 2 => groups... 

But then I realized that my problem was a symptom of not thinking ahead. If I were going to list out all possible unique combinations, why the hell was I randomizing it? Instead, I needed a method that would pick out the groups in the exact same way each time. 

So thinking ahead would have saved me a lot of time though in the end I still learned a lot. Though just for more practice I decided to try make my program into a webapp to get more familiar with sinatra. The final result is a page that would show groups and their random distribution whenever it gets refreshed. Eventually I'd like to make a form where people can simply input the number of groups they want and the people they want to sort.
