# The Search and the Stack

== trace P07 ==
Call: grandparent(tom,X)
  Call: parent(tom,_)
  Exit: parent(tom,liz)
  Call: parent(liz,X)
  Fail: parent(liz,X)
  Redo: parent(tom,liz)
  Exit: parent(tom,bob)
  Call: parent(bob,X)
  Exit: parent(bob,ann)
Exit: grandparent(tom,ann)
answer 1: X=ann
Redo: grandparent(tom,ann)
  Redo: parent(bob,ann)
  Exit: parent(bob,pat)
Exit: grandparent(tom,pat)
answer 2: X=pat
Redo: grandparent(tom,pat)
  Redo: parent(bob,pat)
  Fail: parent(bob,X)
  Redo: parent(tom,bob)
  Fail: parent(tom,_)
Fail: grandparent(tom,X)
== 2 answers ==


Every Call line pushes a frame onto the stack. The first line, Call: grandparent(tom, X), pushes the frame. Then Call: parent(tom, _) pushes a frame one level for the first body goal. Every Exit and Fail line pops a frame. Exit: parent(tom, liz) pops with a result. Fail: parent(liz, X) pops with no result. The last line, Fail: grandparent(tom, X), pops the frame and leaves the stack empty again.

A frame in Activity 15 was thrown away on return. Exit: parent(tom,liz) looks the same, but the goal is not gone. Right after Fail: parent(liz,X), the line Redo: parent(tom,liz) resumes it, and the next line is Exit: parent(tom,bob). It did not rescan the facts from the top. The goal remembered which fact it matched and that more were untried, which is the choice point. It also knew which bindings to undo. The same happens at Redo: parent(bob,ann), which goes straight to Exit: parent(bob,pat).

Swapping the clauses changes whether reach/2 answers because its graph has a cycle, while the family tree in ancestor/2 does not.

In reach/2, putting the recursive clause first makes it keep following the cycle before it ever tries checking the base case,
so it gets stuck in an infinite loop and never gives an answer. 

In ancestor/2, the family tree cannot loop back on itself as following parent/2 eventually reaches someone with no parents and stops, 
so swapping the clauses just checks further ancestors before direct parents and returns the exact same answers in a different order.