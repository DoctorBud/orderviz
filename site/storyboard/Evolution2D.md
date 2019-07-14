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

```
1 2 3 4 5 6
2 1 3 4 5 6
2 3 1 4 5 6
2 3 4 1 5 6
2 3 4 5 1 6
2 3 4 5 6 1
2 3 4 5 6 ?
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
      pairwiseCalculated[i] = i + 2;
      pairwiseCalculated[i + 1] = i + 1;
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

```p5js/playable/autoplay/center

var n = 100;
var cellWidth = 4;
var cellHeight = 4;

p5.setup = function() {
  window.draw2D.setup(p5, cellWidth, cellHeight, n);
};

var nwiseCalculated = null;
function nwise(row) {
  if (!nwiseCalculated) {
    nwiseCalculated = new Array(n);
    for (var i = 0; i < n; ++i) {
      if (true || !nwiseCalculated[i]) {
        nwiseCalculated[i] = 2 * (i + 1);
        nwiseCalculated[i + i + 1] = i + 1;
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
