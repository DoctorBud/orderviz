### The Size of Total Order Construction

Given a countable set $P$, the number of possible total orders  $\lvert P \rvert !$ is much larger than the size of the set.  Not surprisingly, you can't map a set onto it's total orders.  We can construct a simple diagonalization proof to show this.  We'll begin by showing that you can't map the natural numbers $\mathbb{N}$  onto the set of total orders on $\mathbb{N}$.

# --outlinebox theorem1
You can't map the natural numbers $\mathbb{N}$ onto it's  total orders.

**PROOF:**
Let $T$ be the set of all total orders on $\mathbb{N}$. Let $f: \mathbb{N} \rightarrow T$ be a function that maps $\mathbb{N}$ to $T$ and let $f$ be onto.   Since $\mathbb{N}$ is countable and $f$ is onto, we can form a complete list of all the elements of $T$


$$
 \begin{array}{c c c c c c c }
 f(1) & = & \fbox{2}   & 3 & 8  & 9    &  7 \dots                             \\
 f(2) & = & 1   & \fbox{8}  & 7  & 2    &  5  \dots                            \\
 f(3) & = & 9   & 3            & \fbox{2}  & 14  &  11  \dots                  \\
 f(4) & = &10  & 6            & 1            & \fbox{3}    &  4  \dots         \\
 f(5) &= & 49 & 5            & 4            & 1              &  \fbox{9} \dots \\
 \vdots & & & & & & 
 \end{array}
 $$


We can use diagonalization to construct an element of the total orders $T$ that is not on this list.   We'll start with the  total order $P = [1, 2, 3, 4, \dots]$ and we'll walk along the diagonal and permute $P$ as needed so that it is different from every total order on the list.

```python
P = [1,2,3,4, ... ]
for i in [1,2,3,4, ...]:
	if P[i] == f(i)[i]:
		swap(P[i], P[i + 1])

```

Since this new total order $P$ is different from every total order on our list, the function $f$ was not onto.  This is a contradiction and we conclude the function $f$ does not exist. 

# --outlinebox

This proof can be adapted to show that you can't map any finite set $ { 1,2,3, \ldots n } $ with $n > 2$ onto it's total orders.  When we are building up the permutation `P`, we just need some way to manage the case where we get to the end and find that `P[n] == f(n)[n]`.  There is no element `P[n+1]` to swap with `P[n]`.  

If `f(1)[1]` is not equal to `n`, then we can swap `P[1]` with `P[n]`. Now `P[n]` is different from `f(n)[n]` and `P[1]` is different from `f(1)[1]` and our total order `P` is different from all our listed total orders.  Similarly, if `f(2)[2]` is not equal to `n`, then we can swap `P[2]` with `P[n]` and again, `P` will not match any of the diagonal entries.  The only tricky case is when `f(1)[1]` and `f(2)[2]` are both equal to `n` giving us the scenario 
$$
 \begin{array}{c c c c c c}
 f(1)      & = & \fbox{n}   & b           & \ldots  &  \\
 f(2)      & = & c            & \fbox{n}  & \ldots  &  \\
                                                         \\
                                                         \\
 f(\text{n}) & = & 9          & 7           & \ldots  & \fbox{n}  \\
                                                                    \\
 \text{P}  & = & 1            & 2           & \ldots  &  \text{n}          \\
 \end{array}
 $$  

 In this case we will set `P[1]` equal to `n`.  Now we have `f(1)` matching `P` in position $1$.  However, we have `f(2)` different from `P` in position $1$.  We only need to make sure that `P` is different from `f(1)` somewhere. We'll make sure that `f(1)` is different from `P` at position $2$.  To assign `P[2]`, we pick from available values $1$ or $2$ making sure `P[2]` is different from `f(1)[2]`.  Which ever value is left is assigned to `P[n]`, avoiding the match with `f(n)[n]`.  