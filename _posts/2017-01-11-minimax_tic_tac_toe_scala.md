---
layout: post
title:  "Minimax-based AI for Tic-Tac Toe in Scala"
date:   2017-01-11 23:46:00
categories: computer_science
tags:
- algorithms
- functional_programming
comments: true
---

Tic-Tac Toe belongs to the class of games, where two players compete against each other with strictly opposite interests, so the win of one player means the lose of another player. 

![Introduction image](/images/minimax_tic_tac_toe/intro.png)

<!--more-->

## Minimax Principle ##

A rational choice of the actions of the players during such kind of games is based on the maximization of the "chances to win". However, taking into account that there are only two players, the objectives of the players can be redefined in a following way: 
- the first player wants to *maximize its own chances to win*
- the second player wants to *minimize the chances of the opponent to win*

How can the first player choose the best move, given the current state of the game?  
The optimal strategy would be to choose such move, which maximizes the chances to win, *even in case if the second player will make the best possible move afterwards*.

The similar strategy is optimal for the second player as well: it is needed to choose such move, which minimizes the chances of the first player to win, *even if the first player will make the best possible move afterwards*.

Hence, in order to choose the best move, the first player should calculate the chances of win for all possible moves, and in order to evaluate the optimality of each move - it is needed to estimate which move the opponent will do afterwards. However, the opponent will make the decision about the optimal move, based on the similar logic (based on the further optimal decision of the first player), and so forth. 

Thus, the logic of evaluation of the optimality of move can be represented as a tree of game states, where each move represented as an edge, in combination with mutually alternating decisions regarding the optimality, depending on which player makes a move (e.g.: "maximization of chances of the first player to win" -> "minimization of chances of the first player" -> "maximization of chances of the first player" -> ... -> until the end of the game).

Described strategy is known as a [Minimax principle](https://en.wikipedia.org/wiki/Minimax).

Below presented an example of the Minimax tree for the instance of Tic-Tac toe:
![Example of the Minimax tree](/images/minimax_tic_tac_toe/minimax_tree_example.png)

## Minimax Algorithm in Scala ##

Let's implement the described Minimax strategy in Scala.

First of all, let's define the basic components.

1) The trait which represents the state of the game:
{% highlight scala %}
trait State[S <: State[S]] {
  def isGameOver      : Boolean
  def playerOneWin    : Boolean
  def playerTwoWin    : Boolean
  def isPlayerOneTurn : Boolean
  // generates all possible states
  // which can be produced from
  // the current state of the game
  def generateStates  : Seq[S]
}
{% endhighlight %}

2) The trait which represents the algorithm for finding the optimal move for the given state of the game:
{% highlight scala %}
trait BestMoveFinder[S <: State[S]] {
  // produces the state, which represents
  // the most optimal move for the given
  // state of the game
  def move(s: S): S
}
{% endhighlight %}

3) Implementation of the Minimax strategy for finding of the best move:

{% highlight scala %}
class MinMaxStrategyFinder[S <: State[S]] extends BestMoveFinder[S] {

  // Here, and in other methods
  // parameter s - is a current state of the game
  def move(s: S): S = 
    if(s.isPlayerOneTurn) firstTurn(s).state
    else secondTurn(s).state
  
  def secondTurn(s: S): Outcome = 
    bestMoveFinder(minimize, firstTurn, PLAYER_ONE_WIN, s)
  
  def firstTurn(s: S): Outcome = 
    bestMoveFinder(maximize, secondTurn, PLAYER_ONE_LOOSE, s)
  
  def bestMoveFinder(strategy: Criteria, opponentMove: S => Outcome, 
                     worstOutcome: Int, s: S): Outcome = 
    if(s.isGameOver) Outcome(outcomeOfGame(s), s)
    else s.generateStates
          .foldLeft(Outcome(worstOutcome, s)){(acc, state) => {
              val outcome = opponentMove(state).cost
              if(strategy(outcome, acc.cost)) Outcome(outcome, state)
              else acc}}
  
  def outcomeOfGame(s: S): Int = 
    if (s.playerOneWin) PLAYER_ONE_WIN 
    else if(s.playerTwoWin) PLAYER_ONE_LOOSE 
    else DRAW
  
  case class Outcome(cost: Int, state: S)
  
  val PLAYER_ONE_WIN   =  1 // the outcome, when first player wins
  val PLAYER_ONE_LOOSE = -1 // the outcome, when second player wins
  val DRAW             =  0 // the outcome, when nobody wins
  
  type Criteria = (Int, Int) => Boolean
  def maximize: Criteria = (a, b) => a >= b
  def minimize: Criteria = (a, b) => a <= b
}
{% endhighlight %}

As you can see, the algorithm is implemented in a pure functional style without usage of mutable state.

## Tic-Tac Toe in Scala ##

{% highlight scala %}
case class Position(val row: Int, val col: Int)

class TicTacToeState(val playerOnePositions : Set[Position],
                     val playerTwoPositions : Set[Position],
                     val availablePositions : Set[Position],
                     val isPlayerOneTurn : Boolean,
                     val winLength : Int) extends State[TicTacToeState] {
  
  lazy val isGameOver  : Boolean = 
    availablePositions.isEmpty || playerOneWin || playerTwoWin
    
  lazy val playerOneWin: Boolean = checkWin(playerOnePositions)
  
  lazy val playerTwoWin: Boolean = checkWin(playerTwoPositions)
  
  def generateStates: Seq[TicTacToeState] = 
    for(pos <- availablePositions.toSeq) yield makeMove(pos)
            
  def makeMove(p: Position): TicTacToeState = {
    assert(availablePositions.contains(p))
    if(isPlayerOneTurn) 
      new TicTacToeState(
          playerOnePositions + p, 
          playerTwoPositions, 
          availablePositions - p, 
          !isPlayerOneTurn, 
          winLength)
    else new TicTacToeState(
          playerOnePositions, 
          playerTwoPositions + p, 
          availablePositions - p, 
          !isPlayerOneTurn, 
          winLength)}
      
  def checkWin(positions: Set[Position]): Boolean =
    directions.exists(winConditionsSatisified(_)(positions))
    
  final val directions = List(leftDiagonal, rightDiagonal, row, column)
  
  type StepOffsetGen = (Position, Int) => Position
  
  def leftDiagonal : StepOffsetGen = 
    (pos, offset) => Position(pos.row + offset, pos.col + offset)
    
  def rightDiagonal : StepOffsetGen = 
    (pos, offset) => Position(pos.row - offset, pos.col + offset)
    
  def row : StepOffsetGen = 
    (pos, offset) => Position(pos.row + offset, pos.col)
    
  private def column : StepOffsetGen = 
    (pos, offset) => Position(pos.row, pos.col + offset)
    
  def winConditionsSatisified(step: StepOffsetGen)
                             (positions: Set[Position]): Boolean =
    positions exists( position =>
      (0 until winLength) forall( offset =>
        positions contains step(position, offset)))
    
  // Just additional constructor for convenience  
  def this(dimension: Int, winLength: Int) = this(
      Set(), Set(), // positions of the players are empty initially
      (for{row <- (1 to dimension) // initialize available positions 
           col <- (1 to dimension)} yield Position(row, col)).toSet, 
      true, winLength)
}
{% endhighlight %}

{% highlight scala %}
class HumanTicTacPlayer extends BestMoveFinder[TicTacToeState] {
  def move(s: TicTacToeState): TicTacToeState = {
    println("Input the row and the column, please:")
    val (row, col) = readf2("{0, number} {1,number}")
    val pos = Position(row.asInstanceOf[Long].toInt, col.asInstanceOf[Long].toInt)
    try {
      s.makeMove(pos)
    } catch {
      case _: Throwable => {
        println("Such move is impossible!")
        move(s)}}}
}
{% endhighlight %}

{% highlight scala %}
object Demo extends Application {
  
  val dimension = 3
  var game: TicTacToeState = new TicTacToeState(dimension, dimension)
  def display(game: TicTacToeState) = 
    (1 to dimension).map(row =>
      (1 to dimension).map(col => {
        val p = Position(row, col)
        (game.playerOnePositions contains p, game.playerTwoPositions contains p) match {
          case (true, false) => playerOne
          case (false, true) => playerTwo
          case _ => emptyCell
        }
      }).mkString).mkString("\n") + "\n"
  val playerOne = "X"
  val playerTwo = "O"
  val emptyCell = "."
  
  val ai = new MinMaxStrategyFinder[TicTacToeState]()
  val human = new HumanTicTacPlayer()
  
  var players = List(human, ai)
  
  while(!game.isGameOver) {
    val player = players.head
    game = player.move(game)
    println(display(game))
    players = players.tail :+ player
  }
  
  if(game.playerOneWin)      println("Player One win!")
  else if(game.playerTwoWin) println("Player Two win!")
  else                       println("Draw!")
}
{% endhighlight %}

<div id="disqus_thread"></div>
<script>

var disqus_config = function () {
this.page.url = "http://lagodiuk.github.io/computer_science/2016/12/19/efficient_adjacency_lists_in_c.html";
this.page.identifier = "efficient_adjacency_list_implementation";
};

(function() { // DON'T EDIT BELOW THIS LINE
var d = document, s = d.createElement('script');

s.src = '//lahodiuk.disqus.com/embed.js';

s.setAttribute('data-timestamp', +new Date());
(d.head || d.body).appendChild(s);
})();
</script>
<noscript>Please enable JavaScript to view the <a href="https://disqus.com/?ref_noscript" rel="nofollow">comments powered by Disqus.</a></noscript>
