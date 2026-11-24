<!--no-pdf-->
# Why Each Prediction Reads the Way It Does

Write one entry per case, before you run anything. Name the rule behind each
predicted field. One to three sentences is enough. The grader checks the
structure of this file only. It does not judge how good the reasoning is, and
it sets no word minimum.

For every case you predicted wrongly, add a short correction after you run
it: what you predicted, what the language printed, and which rule you
misread. Corrections belong in the correction commits, not the prediction
commit.

Commit this file together with `predictions.tsv`, before any case runs:

```bash
git add predictions.tsv REASONING.md
```

The example below uses a query that is not one of the 16 cases, so it gives
no answer away. `parent(pat, X)` reads `parent(pat, jim)` because the `parent`
facts are tried top to bottom and only that one has `pat` as its first
argument:

```text
## parent(pat, X)

The goal scans the parent facts in source order. Only parent(pat, jim) has
pat first, so X binds to jim. No second fact matches, so the first ; finds
nothing and the search ends after one answer.
```

## P01
The goal scans the parent facts in order and sees parent(tom, liz) first then parent(tom, bob) next after doing ;, given 
the first argument tom. Hence, it binds X to liz first then to bob next. Only these two matches tom so count is 2.

## P02

parent(tom, bob) checks whether Tom is a parent of Bob. Since this fact exists in the knowledge base, Prolog succeeds with true. There are no additional solutions, so the count is 1.

## P03

The goal scans the parent facts in order. In here, none matches to be Tom's parent so first is false, second is none 
and count is 0.

## P04

X = f(Y, a), Y = b performs unification from left to right. First, X becomes f(Y, a), then Y is unified with b, resulting in X=f(b,a), Y=b. There is only one solution.

## P05

We apply unification in here provided the =. So here, X binds to a and Y binds to b, making first X=a,Y=b. Since it 
already succeeds right away with no other matches, the second is none and the count is 1.

## P06

a = b attempts to unify two different atoms. Since the atoms are not identical, unification fails, resulting in false with zero solutions.

## P07

The main goal unifies G with tom and C with X in the grandparent rule, transforming the two subgoals into parent(tom, P)
and parent(P, X). Evaluating left to right, the first subgoal initially binds P to liz, which fails in the second subgoal
since liz has no children, forcing Prolog to backtrack and instead bind P to bob. Proceeding to the second subgoal with P
as bob first matches parent(bob, ann) to give first as X=ann, and backtracking via ; finds parent(bob, pat) to give second 
as X=pat before running out of matches with count as 2.

## P08

ancestor(tom, X) searches for all descendants of Tom using the recursive ancestor predicate. Prolog finds six solutions through its facts and recursive rules.

## P09

Scanning top to bottom, the main goal first enters rule 1 by unifying A with tom and D with X, skipping liz since she 
has no children and matching parent(tom, bob) to recurse down through ann to sue, where ancestor_r(sue, X) fails and
backtracks to bind sue via rule 2 for first as X=sue. Doing ; backtracks into bob's next child pat and recurses down to
jim the same way for second as X=jim, before unwinding out of rule 1 to find X=ann and X=pat. Once rule 1 is exhausted, 
Prolog moves to rule 2 and unifies ancestor_r(tom, X) with parent(tom, X) to bind X to liz and bob, making count 6.

## P10

ancestor(X, jim) searches for all ancestors of Jim. The total count is 3.

## P11

The main goal scans reach rules which contains 2 and the first one being recursion; first, by unification 
we bind X to a and Y to d which gives reach(a, d):- reach(a, Z), edge(Z, d). Since this is left recursive we'll end up
being in an infinite loop. Thus, it never reaches the second subgoal nor backtracks and tries 2nd rule, and what will 
eventually happen is that the search will run past its budget before having the answer which makes first, second, and
count to be unbounded.

## P12

reach_r(a, d) checks whether node d is reachable from node a using the recursive reachability predicate. The query succeeds, and because the recursive definition can repeatedly generate solutions, the count is unbounded.

## P13

The main goal scans the len rules top-to-bottom, skipping the first rule since the list is not empty and unifying with 
len([_|T], N) :- len(T, M), N is M + 1 to ignore the head a and bind T to [b, c]. Evaluated left to right, len([b, c], M) 
recursively removes each head until the tail becomes empty and hits the base case len([], 0) to bind M2 to 0, then 
unwinds back up through M1 is 0 + 1 = 1 and M is 1 + 1 = 2 to finally compute N is 2 + 1 = 3 for first as N=3. Doing ; 
backtracks to the empty list where the recursive rule fails since it cannot split an empty list, which is why second is none 
and count as 1.

## P14

join(X, Y, [1, 2]) divides the list [1, 2] into two lists. Prolog uses backtracking to find three possible divisions: ([], [1,2]), ([1], [2]), and ([1,2], []).

## P15

Since the prolog runs from left to right, it will ask x > 2 first. And since X still is unbounded which Prolog throws an
instantiation error. Hence first, second, and count are all error.

## P16

elem(X, [1, 2, 3]), X > 2 first finds elements in the list and then checks whether each is greater than 2. Only X=3 satisfies the comparison, resulting in one solution.

