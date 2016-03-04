---
layout: post
title: "Hello World in IntelliJ - First Experience with an IDE"
date: 2016-03-04 01:28:57 -0500
comments: true
categories:
- Practice
- IntelliJ
- Java
- Hello World
---
I come from a coding bootcamp background (I attended Flatiron School). Being in the web development class, my first language was Ruby and my coding environment was pretty much just Sublime Text (though more recently Atom). With Ruby as a first language, IDEs can seem to be pretty alien. What's the point? Isn't everything I need just in a simple text editor? These are the questions I had when Ruby was the only language I knew. While there are Ruby IDEs out there, you can get by with no trouble just using a text editor.

I ran into my first feeling that I needed an IDE when I started learning Java. It was very different from Ruby. I needed to get used to a static language. Arrays that needed to know what size they were, variables to be declared and remembering to compile a program before running it. I admit that my first Java program (a simple shopping cart reader that would get its input from a text file and give a receipt as an output after calculating item totals and tax) was programmed entirely on sublime and while coding, I kept thinking - This feels a lot harder than it should be. After the horrified look I got from a friend when I mentioned I was coding Java on a text editor, he recommended IntelliJ as an IDE to use. And that is where I am now.

So to start off... what is an IDE?

IDE stands for Integrated Development Environment. IntelliJ doesn't just give a platform for writing code on, but it also contains a compiler, built-in debugging (way better than my beloved binding.pry), and a very helpful autocomplete.

This all seems great!

One of my main issues from my first Java program was just forgetting to compile and so on... it was an extra step that I didn't need to do with Ruby. However, just getting started on an IDE having never encountered one before has led me to see that there is a learning curve. With a text editor, I could hit the ground running and immediately start writing code. But there is some familiarity that needs to be developed with an IDE and learning one is a challenge in and of itself.

As you can see from the title of this blog post, this is just a simple guide on creating a Hello World program with IntelliJ. (The pictures might look a little bit small but they expand if you click on them.) This post also helps me document my learning progress.

Step 1: Create a new project

When you open up IntelliJ, click on the option to make a new project.
<center>
<img src="{{ root_url }}/images/intellij1.png" width = 400>
</center>
Step 2: Navigate through the options and select the command line app template.

Once you've clicked new project, you are given the option to include additional libraries and frameworks. If you click next (because we're not using any), you'll get to the template selection page. We're just making a hello world program so we'll just pick the command line app template.
<center>
<img src="{{ root_url }}/images/intellij2.png" width = 400><img src="{{ root_url }}/images/intellij2a.png" width = 400>
</center>
Step 3: Name the project and choose its location

I already had a folder called HelloWorld made.
<center>
<img src="{{ root_url }}/images/intellij3.png" width = 400>
</center>
Step 4: Write the code

Selecting the command line template makes the public static void appear automatically. So we just need to fill in the rest. While writing, it was nice to see the auto-complete. It's helpful to be able to see the methods you have available to you.
<center>
<img src="{{ root_url }}/images/intellij4.png" width = 400>
</center>
Step 5: Running the code

To run the code and see "hello world!" printed out, we just need to hit the green triangle on the top right of the window.
<center>
<img src="{{ root_url }}/images/intellij5.png" width = 400>
</center>
And there you have it, my first attempt at trying to familiarize myself a little bit with the absolute basics of IntelliJ before really diving into it. It was definitely a little strange for me at first, coming over from Ruby for the need of an IDE. But I can already tell that if I had written my Java shopping cart reader with IntelliJ, it would have saved me a lot of time in the long run. Overall, I can tell that using an IDE is a lot more powerful but it does have a steeper learning curve when compared to the text editors that I am more familiar with.
