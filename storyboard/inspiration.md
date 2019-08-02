---
impress:
  data-x: -600
  data-y: 0
---

## Inspiration

There are two well know proofs that are the inspiration for this work.  First is the well known HALTING problem proof.  The second is the proof that decidable languages are exactly those that are recognizable in standard string order.

### The HALTING problem



# --outlinebox halt_proof

**PROOF:**

Let's start with the assumption that we have a machine called DECIDER that takes a machine, input pair $<M,w>$ and decides whether the machine $M$ accepts string $w$.

On input $<M,w>$, DECIDER accepts $<M,w>$ if $M$ accepts $w$ and rejects $<M,w>$ if $M$ does not accept $w$. DECIDER has the capacity to recognize the case where $M$ fails to halt on $w$ and reject $<M,w>$ in that case.  This give it the ability to decide all languages that can be recognized by a Turing machine $M$.    

If such a machine existed, we could use it to define the following machine which we'll call DIAGONAL.  DIAGONAL takes a Turing machine $M$ as input.  It then makes a call to subroutine DECIDER.  It gives DECIDER the input $<M, M>$ asking whether machine $M$ accepts it's own description.  DIAGONAL will take the result of this query and output the opposite.  If $M$ accepts it's own description, then DIAGONAL will reject $M$.  If $M$ fails to accept it's own description, then DIAGONAL will accept it.

Now that we have our description of DIAGONAL, we can ask what happens if we call DIAGONAL with it's own description as an input.  What will happen?  DIAGONAL will feed the input < DIAGONAL, DIAGONAL > to the subroutine DECIDER.  If DIAGONAL accepts it's description, then it will reject it's descripiton.  Similarly, if DIAGONAL rejects it's own description it must then accept it.  This is a contradiction and we must conclude that the machine DECIDER doesn't exist.  


# --outlinebox

### Dicidable Languages and Standard String Order

# --outlinebox standard_order_proof

# --outlinebox

**PROOF:**



# --outlinebox
---

[Home](:@Home)
