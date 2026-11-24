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

Write one to three sentences here. Name the rule that fixes each field.

## P02

parent(tom, bob) checks whether Tom is a parent of Bob. Since this fact exists in the knowledge base, Prolog succeeds with true. There are no additional solutions, so the count is 1.

## P03

Write one to three sentences here. Name the rule that fixes each field.

## P04

X = f(Y, a), Y = b performs unification from left to right. First, X becomes f(Y, a), then Y is unified with b, resulting in X=f(b,a), Y=b. There is only one solution.

## P05

Write one to three sentences here. Name the rule that fixes each field.

## P06

a = b attempts to unify two different atoms. Since the atoms are not identical, unification fails, resulting in false with zero solutions.

## P07

Write one to three sentences here. Name the rule that fixes each field.

## P08

ancestor(tom, X) searches for all descendants of Tom using the recursive ancestor predicate. Starting with X=liz and followed by X=bob, Prolog finds six solutions,.

## P09

Write one to three sentences here. Name the rule that fixes each field.

## P10

ancestor(X, jim) searches for all ancestors of Jim. Prolog finds three solutions, beginning with X=pat and followed by X=tom. The total count is 3.

## P11

Write one to three sentences here. Name the rule that fixes each field.

## P12

reach_r(a, d) checks whether node d is reachable from node a. The query succeeds, and because recursive can repeatedly generate solutions, the count is unbounded.

## P13

Write one to three sentences here. Name the rule that fixes each field.

## P14

join(X, Y, [1, 2]) divides the list [1, 2] into two lists. Prolog uses backtracking to find three possible divisions: ([], [1,2]), ([1], [2]), and ([1,2], []).

## P15

Write one to three sentences here. Name the rule that fixes each field.

## P16

elem(X, [1, 2, 3]), X > 2 first finds elements in the list and then checks whether each is greater than 2. Only X=3 satisfies the comparison, resulting in one solution.

