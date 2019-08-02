---
impress:
  data-x: 600
  data-y: 0
---

## Orderings in a Wasteful 2D Matrix

Similar to 1D Cellular Automata

One way to visualize different orderings is to use a single row of a 2D matrix to represent a generated element of the set, and to list these rows in order vertically. In effect, we are showing generation order vertically, and using the single highlighted element in a row to indicate the element's index. Our *human* eye may be able to see patterns in the evolution of the ordering, or at least some pretty pictures.


### A Simple Drawing Function

We'll define a Javascript *convenience function* that can handle the drawing of a 2D matrix, and well give it some parameters to specify the size of the matrix, and to customize the ordering function.

```p5js/playable/autoplay/center/debug
const margin = 10;

function setup2D(p5, cellWidth, cellHeight, n) {
  p5.frameRate(1);
  p5.createCanvas(n * cellWidth + 2 * margin, n * cellHeight + 2 * margin);
};

function draw2D(p5, cellWidth, cellHeight, n, getSelectedCellForRow) {
  p5.background('darkslateblue');
  p5.stroke('lightgreen');
  p5.strokeWeight(0.5);

  for (var row = 0; row < n; ++row) {
    const rectTop = row * cellHeight;
    p5.noFill();
    p5.rect(0 + margin, rectTop + margin, n * cellWidth, cellHeight);
    p5.fill('lightblue');
    var selectedCell = getSelectedCellForRow(row);
    if (selectedCell < n) {
      p5.rect(selectedCell * cellWidth + margin, rectTop + margin, cellWidth, cellHeight);
    }
  }

  p5.noStroke();
};
window.draw2D = {
  setup: setup2D,
  draw: draw2D,
};
```

### The Identity Ordering

The simplest ordering is none at all, where each element is emitted in its *natural* order.

The *trace* of this execution is fairly boring:

```
 0: 
 1: 1
 2: 1 2
 3: 1 2 3
 4: 1 2 3 4
...
```


```p5js/playable/autoplay/center
var n = 20;
var cellWidth = 10;
var cellHeight = 10;

p5.setup = function() {
  window.draw2D.setup(p5, cellWidth, cellHeight, n);
};

function identity(row) {
  return row;
};

p5.draw = function() {
  window.draw2D.draw(p5, cellWidth, cellHeight, n, identity);
};
```


### The Non-overlapping Pairwise Swap Ordering

This is the permutation

$$\left((1,2)(3,4)\dots\right)$$

The effect of the `pairwise` function below is to swap pairs of the form $(2n + 1, 2n + 2)$ (an odd number followed by an even number), so that once a pair is swapped, the resulting swapped number is not considered in any further computation.


The trace for this computation shows how we are *skipping* a transition in order to have a non-overlapping swap. This requires a little more complexity in our algorithm, but does not require unbounded space.


```
 0: 1 2
 1: 2 1
 2: 2 1
 3: 2 1 4 3
 4: 2 1 4 3
...
```



```p5js/playable/autoplay/center
var n = 20;
var cellWidth = 10;
var cellHeight = 10;

p5.setup = function() {
  window.draw2D.setup(p5, cellWidth, cellHeight, n);
};

function pairwise(row) {
  return row % 2 === 0 ? (row + 1) : (row - 1);
};

p5.draw = function() {
  window.draw2D.draw(p5, cellWidth, cellHeight, n, pairwise);
};

```



### The Overlapping Pairwise Swap Ordering

Unlike the above algorithm, we'll do pairwise swaps for each element of the natural order, rather than skipping. This means that the result of a swap will be used in subsequent computation.

Overall, the result is boring. All it does is shift the entire sequence left one position, and leaves a $1$ at the end of the potentially infinite sequence.

The trace from the algorithm that generates this ordering uses bounded memory; we need to remember the next cell's value so that we can *bucket brigade* it forward.


```
0: 1 2 3 4 5 6
1: 2 1 3 4 5 6
2: 2 3 1 4 5 6
3: 2 3 4 1 5 6
4: 2 3 4 5 1 6
```


```p5js/playable/autoplay/center
var pairwiseCalculated = null;

var n = 20;
var cellWidth = 10;
var cellHeight = 10;

p5.setup = function() {
  window.draw2D.setup(p5, cellWidth, cellHeight, n);
};

function pairwise(row) {
  if (!pairwiseCalculated) {
    pairwiseCalculated = new Array(n);
    for (var i = 0; i < n; ++i) {
      pairwiseCalculated[i] = i + 1;
      pairwiseCalculated[i + 1] = i;
    }
  }

  return pairwiseCalculated[row];
}

p5.draw = function() {
  window.draw2D.draw(p5, cellWidth, cellHeight, n, pairwise);
};

```



### The N-wise Swap Ordering

This is a more dynamic version of the pairwise swap. In this case, we'll be doing a pairwise swap for each new element, but we will not affect an element that has already been swapped.

We should expect a *trace* like:

```
1:  1  2  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18
2:  2  1  3  4  5  6  7  8  9 10 11 12 13 14 15 16 17 18
3:  2  1  6  4  5  3  7  8  9 10 11 12 13 14 15 16 17 18
4:  2  1  6  8  5  3  7  4  9 10 11 12 13 14 15 16 17 18
5:  2  1  6  8  10 3  7  4  9 5 11 12 13 14 15 16 17 18
6:  2  1  6  8  10 3  7  4  9 5 11 12 13 14 15 16 17 18
7:  2  1  6  8  10 3  14  4  9 5 11 12 13 7 15 16 17 18
8:  2  1  6  8  10 3  14  4  9 5 11 12 13 7 15 16 17 18
9:  2  1  6  8  10 3  14  4  18 5 11 12 13 7 15 16 17 9
10: 2  1  6  8  10 3  14  4  18 5 11 12 13 7 15 16 17 9
11: 2  1  6  8  10 3  14  4  18 5 22 12 13 7 15 16 17 9
12: 2  1  6  8  10 3  14  4  18 5 22 24 13 7 15 16 17 9
13: 2  1  6  8  10 3  14  4  18 5 22 24 26 7 15 16 17 9
14: 2  1  6  8  10 3  14  4  18 5 22 24 26 7 15 16 17 9
15: 2  1  6  8  10 3  14  4  18 5 22 24 26 7 30 16 17 9
16: 2  1  6  8  10 3  14  4  18 5 22 24 26 7 30 16 17 9
17: 2  1  6  8  10 3  14  4  18 5 22 24 26 7 30 16 34 9
18: 2  1  6  8  10 3  14  4  18 5 22 24 26 7 30 16 34 9
```

The above trace represents the internal machine states that generate the first 18 numbers in the sequence. The 18th row represents the first 18 rows of a matrix diagram:

`2  1  6  8  10 3  14  4  18 5 22 24 26 7 30 16 34 9`

But the trace itself is *not* visible in the matrix diagram.


```p5js/playable/autoplay/center

var n = 100;
var cellWidth = 5;
var cellHeight = 5;

p5.setup = function() {
  window.draw2D.setup(p5, cellWidth, cellHeight, n);
};

var nwiseCalculated = null;
function nwise(row) {
  if (!nwiseCalculated) {
    nwiseCalculated = new Array(n);
    for (var i = 0; i < n; ++i) {
      if (typeof nwiseCalculated[i] === 'undefined') {
        nwiseCalculated[i] = 2 * (i + 1) - 1;
        nwiseCalculated[2 * (i + 1) - 1] = i;
      }
    }
  }

  return nwiseCalculated[row];
}

p5.draw = function() {
  window.draw2D.draw(p5, cellWidth, cellHeight, n, nwise);
};
```


---

[Home](:@Home)
