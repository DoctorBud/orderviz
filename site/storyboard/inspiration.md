---
impress:
  data-x: -600
  data-y: 0
---

## Inspiration

There are two well know proofs that are the inspiration for this work.    The first is the proof that decidable languages are exactly those languages that are recognizable in standard string order. The second is the well known HALTING problem proof.



### Dicidable Languages and Standard String Order

# --outlinebox standard_order_proof

A language is decidable if and only if some enumerator enumerates the language in lexicographic order.

**PROOF:**

Assume that $L$ is a decidable language.  There must be a machine $M$ that decides $L$.  We can use $M$ to construct an enumerator $E$ that enumerates $L$ in lexicographical order.  $E$ will move through all possible strings in lexicographical order starting with the empty string.  It will run $M$ on each string.  If $M$ accepts a string it will output it.  Then it will move on to the next lexicographically greater string and test that.  Since $M$ is decidable, all subroutine calls to $M$ will halt with accept or reject.  Thererore $E$ enumerates $L$ in lexicographic order.

Now we'll assume we have an enumerator $E$ for language $L$ that enumerates $L$ in lexicographic order.  We can use $E$ to create a machine $M$ that decides $L$.  Machine $M$ will input a string $w$.  It will then call enumerator $E$.  If $E$ outputs $w$ then $M$ will accept $w$ and halt.  Otherwise, $M$ will halt and reject $w$ as soon as $E$ outputs some string that is lexicographically larger than $w$.

It follows that a language $L$ is decidable if and only if some enumerator enumerates it in lexicographical order.

# --outlinebox

### Order Matters

We tend to think of languages as unordered sets. This is appropriate when we want to talk about languages in general.  However, this proof hints that order can be an important property for recognizable sets. Given an infinite recognizable language, we can't always generate it in whatever order we want.  In fact, we'll see that for any infinite recognizable set, most of it's orderings can't be generated.  


# :::: haltingproblem

### The HALTING problem

# --outlinebox halt_proof

The language

$$ A_{TM} = \{ <M,w> | M \; \textrm{is a Turing machine and} \; M \; \textrm{accepts}  \; w \}$$

is undecidable.

**PROOF:**

Let's start with the assumption that we have a machine called $H$ (for halt) that decides the language $A_{TM}$.  It takes a machine, input pair $<M,w>$ and decides whether the machine $M$ accepts string $w$.

On input $<M,w>$, $H$ accepts $<M,w>$ if $M$ accepts $w$ and rejects $<M,w>$ if $M$ does not accept $w$. $H$ has the capacity to recognize the case where $M$ fails to halt on $w$ and reject $<M,w>$ in that case.    

If such a machine existed, we could use it to define the following machine which we'll call $D$ for diagonal.  $D$ takes a Turing machine $M$ as input.  It then makes a call to subroutine $H$.  It gives $H$ the input $<M, M>$ asking whether machine $M$ accepts it's own description.  $D$ will take the result of this query and output the opposite.  If $M$ accepts it's own description, then $D$ will reject $M$.  If $M$ fails to accept it's own description, then $D$ will accept it.

Now that we have our description of $D$, we can ask what happens if we call $D$ with it's own description as an input.  In other notation, what is $D(< D >)$?  $D$ will feed the input $< D, D >$ to the subroutine $H$.  If $D$ accepts it's description, then it will reject it's descripiton.  Similarly, if $D$ rejects it's own description it must then accept it.  This is a contradiction and we must conclude that the machine $H$ doesn't exist and the language $A_{TM}$ is undecidable.


# --outlinebox

### The No Passing Conjecture
# --outlinebox nopassing
	
	There are 
# --outlinebox

# :::: 
---

[Home](:@Home)
