---
layout: post
title: "Some TDD Practice"
date: 2016-02-22 13:28:26 -0500
comments: true
categories:
- TDD
- RSpec
- Practice
---

Today I'm going to be practicing my TDD by implementing a set in Ruby using arrays. While the Set class in Ruby uses hashes as storage for various reasons, this is just for practicing a TDD way of coding and also practice for writing tests in RSpec. So I'm not trying to demonstrate a great way of implementing sets or that this is a better way to do sets (spoiler: It's really not).

<strong>Some things to know about Sets:</strong>
<br>
- Order doesn't matter (unordered)
<br>
- Contains unique values

So the TDD steps... you would want to write tests first and let that guide how you write your code. You would first write a simple test, run it, see it fail and then write minimal code to make it pass (The simplest way you can make it pass). Then you move on to write the next test.

First things first, we are going to write a test. We are going to say what the test is describing. It will be something like this:

```
describe Set do

end
```
Now we are going to think about what kind of method we want the set to have. 

For a quick example, let's say you want to convert the set into an array. So we are going to write a test for that.

```
describe Set do

  describe "#to_array" do
      it "returns an array of all elements in the set" do
        @set = Set.new([1])
        expect(@set.to_array).to contain_exactly(1)
      end
  end

end
```
Note: This test is failing

<img src="{{ root_url }}/images/testfail.png">

(You can ignore the number "3)" there. I screenshotted the fail message only after I finished this exercise so I already have a couple tests written)

As we haven't really written anything, the test will fail when we run it which is exactly what we want. Now we have to write some code to make the test pass. 

```
class Set

  def initialize(array)
    @array = array
  end

  def to_array
    @array
  end

end
```
With this, the test now passes. Great!

Now, we can add more stuff in. Let's write another test to make sure only arrays can get passed as arguments. As Ruby is not a type language, we cannot specify what data types we accept as an argument.

```
it "accepts only an array as an argument" do
    expect(Set.new([1,2,3]).to_array).to contain_exactly(1,2,3)
    expect(Set.new(1).to_array).to contain_exactly()
    expect(Set.new("a").to_array).to contain_exactly()
end
```

The test expects that if the argument is not an array, a set is made but it will be an empty set. It's important to note that our task is to write tests that fail. So even though the first expect line we write will pass, the other two will not. 

To make this next step pass:
```
def initialize(array)
  if array.class != Array
    @array = []
  else
    @array = array
  end
end
```
Now we can worry about one of the definitions of a set - that it contains only unique values. And we can write a test for that.
```
it "will create a set with unique elements" do
    expect(Set.new([1, 1, 2]).to_array).to contain_exactly(1, 2) 
end

```
To make this pass, we can call .uniq on the argument like so:
```
def initialize(array)
  if array.class != Array
    @array = []
  else
    @array = array.uniq
  end
end
```
.uniq might not be the best way to do things, but it is important to keep in mind that at the moment, we are just concerned with doing the minimal to make the test pass.

I won't go through each and every one of my tests but eventually I found myself creating new sets for each test. So in my test file, I ended up doing:

```
before :each do
  @shortset = Set.new([1])
  @mediumset = Set.new([1, 2, 3, 4, 5, 6].shuffle)
  @mediumset2 = Set.new([4, 5, 6, 7, 8, 9].shuffle)
end
```
This way I won't have to keep making new tests. Before each means that before each test, that block of code will run. So less repetition! I added the .shuffle because sets are not ordered. Before that, I had put in arrays in numerical order and didn't realize some tests actually failed when numerical order was not in place. Ideally, I think something I would change are the names of the sets as they aren't very descriptive. There's also a bit of refactoring to be done but the idea of TDD is to do the minimum to pass the tests and then write more tests that you expect to fail.

Overall, there's still some refactoring to be done but I quite like the practice I got with writing tests before the actual coding. I like TDD because it forces you to think before you code. You have to think about what you will be doing. And once the tests pass (there really is no better feeling...), you know that your job is done.

I'm still tinkering around with it (with refactoring and such) but the full code can be viewed on github: <a href = "https://github.com/heatherlim/TDDPractice">HERE</a>