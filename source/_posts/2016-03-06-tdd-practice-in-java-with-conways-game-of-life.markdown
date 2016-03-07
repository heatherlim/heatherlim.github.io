---
layout: post
title: "TDD Practice: Life and Death of a Cell (Conway's Game of Life)"
date: 2016-03-06 23:18:22 -0500
comments: true
categories:
- TDD
- Practice
- Java
- Conway's Game of Life
---
I got a bit more practice since my last post on just barely touching IntelliJ. I'm currently working on implementing Conway's Game of Life in Java and I decided to use TDD to help me along the process. I like how it helps me to think on the design of my code. And that I can be sure new methods I write don't break my previous ones. Right now, I'm going to walk through the process of the cell logic in the Game of Life.

<strong>Rules (Taken from Wikipedia)</strong><br>
1) Any live cell with fewer than two live neighbours dies, as if caused by under-population.<br>
2) Any live cell with two or three live neighbours lives on to the next generation.<br>
3) Any live cell with more than three live neighbours dies, as if by over-population.<br>
4) Any dead cell with exactly three live neighbours becomes a live cell, as if by reproduction.

The first test I decided to write was that a cell's initial state would be dead. (Something nice about using IntelliJ was that it would help auto-import libraries and the auto-completion is really convenient. Also, clicking option+enter brings up available suggestions to fix an error in your code.)
```
public class CellTest {
    @Test
    public void InitialStateisDead() {
        Cell cell = new Cell();
        Assert.assertEquals(cell.getState(), false);
    }  
}
```
This test would automatically fail. Which is what we expected. The next step is to pass this test in the absolute simplest way. So to make this test pass, we would just make the method state return false.
```
public class Cell {
     public boolean getState(){
       return false;
    }
}
```
Great, so now we have all our tests passing. But what happens if the state of our cell is alive? We need to write a test that can check that.
```
@Test
    public void CellStateCanChange(){
      Cell changeCellAlive = new Cell();
        changeCellAlive.changeState();
        Assert.assertEquals(changeCellAlive.getState(), true);
    }
```
The test right now is failing. We need to write a changeState method which is missing right now. While it should make the state of our cell toggle, we are going one small step at a time. I made an instance variable that returns false and made it so our method state would be returning that variable. So the simplest way to just make the test pass would be for changeState to just set the state of the cell as true as you can see in line 7.
```
public class Cell {
    private boolean state = false;
    public boolean getState(){
       return state;
    }
    public void changeState() {
        state = true;
    }
}
```
Now however, we want to see if a cell can change from alive to dead. We will write a test that will try to change the state of an alive cell and the state should return false. Of course the test will fail as right now, changeState can only make the state change to true, not false.
```
@Test
public void CellStateCanChange(){
  Cell changeCellAlive = new Cell();
  changeCellAlive.changeState();
  Cell changeCellDead = new Cell();
  changeCellDead.changeState();
  changeCellDead.changeState();
  Assert.assertEquals(changeCellAlive.getState(), true);
  Assert.assertEquals(changeCellDead.getState(), false);
}
```
To make this test pass, I am going to edit the changeState method. It will check the state of the cell and be able to toggle it.
```
public class Cell {
  private boolean state = false;
  public boolean getState(){
     return state;
  }
  public void changeState() {
     state = !state;
  }
}
```
The test passes now. I want to start incorporating some of the rules of Conway's Game of Life. Namely, the condition where a cell can live onto the next stage. A cell that has two or three neighbours will be able to live onto the next generation.
```
@Test
   public void CanLiveOnToNextStage(){
     Cell cell = new Cell();
       Cell cell2 = new Cell();
       cell2.nextState(3);
       cell.nextState(2);
       Assert.assertEquals(cell.getNextState(), true);
       Assert.assertEquals(cell.getNextState(), true);
   }
```
To make the test pass, I'll create the method getNextState and nextState. nextState determines what the cell's next state is. It will accept an integer as an argument. The integer is meant to represent the number of neighbours that the cell has. getNextState will be retrieving what the next state is.
```
public class Cell {
    private boolean state = false;
    private boolean nextState = false;
    public boolean getState(){
       return state;
    }
    public void changeState() {
        state = !state;
    }
    public boolean getNextState(){
        return nextState;
    }
    public void nextState(int i) {
        if(i == 2 || i == 3){
            nextState = true;
        }
    }
}
```
Now that the tests pass, I'm going to move onto other tests that see what situations a cell wouldn't be living onto the next generation. Let's do under-population. If a live cell has fewer than two live neighbours, it will die. By default however, because our nextState is set to false, our tests should pass automatically. But as we want the tests to fail, I set the next state to have neighbours of 3 first. Then, I set it to lower numbers. To clarify, let's say I did not previously set the state of the neighbours to have 3 first. Then I would end up with a test like this:
```
@Test
    public void WillDieOfUnderpopulation(){
        Cell noNeighbourCell = new Cell();
        Cell oneNeighbourCell = new Cell();
        noNeighbourCell.nextState(0);
        oneNeighbourCell.nextState(1);
        Assert.assertEquals(noNeighbourCell.getNextState(), false);
        Assert.assertEquals(oneNeighbourCell.getNextState(), false);
    }
```
These tests would actually be passing. But that would be an inaccurate reflection of what I want the code to do. I need the tests to fail so I can edit the code to account for that. After all, the next state of a cell is by default always going to be false. I will have to write something to make it true and see if it can change to false. So notice the two extra lines I write on the test below.
```
@Test
    public void WillDieOfUnderpopulation(){
        Cell noNeighbourCell = new Cell();
        Cell oneNeighbourCell = new Cell();
        noNeighbourCell.nextState(3);
        noNeighbourCell.nextState(0);
        oneNeighbourCell.nextState(3);
        oneNeighbourCell.nextState(1);
        Assert.assertEquals(noNeighbourCell.getNextState(), false);
        Assert.assertEquals(oneNeighbourCell.getNextState(), false);
    }
```
To pass this, I just need to fill in the else statement of our nextState method.
```
public void nextState(int i) {
        if(i == 2 || i == 3){
            nextState = true;
        } else {
            nextState = false;
        }
    }
```
I continue to test in this manner. I get the test to pass then I think of more specific scenarios to test to get the test to fail and then I go back to refine the code more. I still need to write the tests for what happens in the event of over-population and how reproduction works.

I also decided to go back to refactor a bit. I began to think that holding the next state of my cell in a variable is a little unnecessary. I delete my getState method as well as the variable. Instead, these are what my methods look like now:
```
public class Cell {
    private boolean state = false;
    public boolean getState(){
       return state;
    }
    public void changeState() {
        state = !state;
    }
    public boolean nextState(int i) {
        if(i == 2 || i == 3){
            return true;
        } else {
            return false;
        }
    }
    public void next(int i) {
        if(state != this.nextState(i)){
            this.changeState();
        }
    }
}
```
I do change the tests a little to reflect this change. Some methods after all are now accepting arguments. It does show my tests could have been testing too much and were a little brittle. Generally, I would not want to change my tests to account for this refactoring. This project is still a work in progress but it feels good to familiarise myself with Java, an IDE as well as testing. The final product will also use the Slick2D library for drawing out this game. I might do another separate blog post on how to integrate it with IntelliJ. While Maven did a lot of it for me, there were some native dependencies that were missing and I had to manually put them in.
