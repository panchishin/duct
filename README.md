# DUCT overview for MCTS

# Simultaneous adverserial search

First, let's review a maximization tree search then we will turn it into a simultaneous maximization tree search.

# Single Agent non-adverserial review

For a single agent, if we are using an Upper Confidence Tree to try and maximize a value
we would make something like MCTS but non-adverserial.

The top node would have a list of children, along with the visits and win/loss.
During backprop, we update the visits, win, and loss of the parents are we go up the tree.
Each parent (other than the top node) is a child in a list.

When we are selecting we simply look at all the children in the list and pick the one with the highest UCB

Now with that review, let's move on

# Two Agent simultanious movement adverserial search

The main change is instead of having a list of children for our 1 agent,
we will keep a table of children, where the columns represent the action of agent X
and the rows represent the action of Y.

During selection, agent X will sum the results by column to decided their action.
This means summing all the visits and win/loss of all the children per column.
Agent Y will do the same to chose their action, but will sum by rows.

## Here is an example

```
| action | attack      | dodge        |
| attack | child id 78 | child id 79  |
| dodge  | child id 80 | child id 81  |
```

In this example the parent has this table instead of a list.
We choose the action for agent X by calculting the UCBs like so
```
attack UCB = UCB( visits = id 78 visits + id 80 visit , wins = id 78 wins + id 80 wins)
dodge  UCB = UCB( visits = id 79 visits + id 81 visit , wins = id 79 wins + id 81 wins)
```
let's say using this the UCB eval for dogde is higher, so agent X chooses dodge.
We do the same for agent Y.  Let's say that agent Y chooses attack.

Since agent X action is dodge, and agent Y action is attack, that means
we select child id 79

Backprop is no different than regular MCTS.
