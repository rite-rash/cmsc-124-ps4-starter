# The Search and the Stack

Replace this paragraph after your checks pass. Paste the output of `./trace`
for one case. Then answer three questions about it.

- Which of its lines push a frame onto Activity 15's runtime stack? Which
  lines pop one?
- What does a `Redo` line resume that a returned frame cannot?


Swapping the clauses changes whether reach/2 answers because its graph has a cycle, while the family tree in ancestor/2 does not.

In reach/2, putting the recursive clause first makes it keep following the cycle before it ever tries checking the base case,
so it gets stuck in an infinite loop and never gives an answer. 

In ancestor/2, the family tree cannot loop back on itself as following parent/2 eventually reaches someone with no parents and stops, 
so swapping the clauses just checks further ancestors before direct parents and returns the exact same answers in a different order.