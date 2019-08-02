---
impress:
  data-x: 600
  data-y: 0
---

## Whisker Diagrams

In Evolution2D, we looked at 2D Matrix diagrams that illustrate the ordering of a generated set. But there are problems with that technique:

- Very wasteful of ink/space
- A diagram can be built that has no meaningful interpretation in ordering-space.


In this document, we'll look at a more concise representation of orderings of $\mathrm{N}$ that is designed to make complexity and pattern in the ordering more visible, while eliminating a lot of ink.


### A Simple Drawing Function

We'll define a Javascript *convenience function* that can handle the drawing of a *whisker diagram*, and well give it some parameters to specify the size of the diagram, and to customize the ordering function.

```javascript /playable/autoplay/center
const margin = 10;

function setupWhisker(p5, cellWidth, cellHeight, n) {
  p5.frameRate(1);
  p5.createCanvas(n * cellWidth + 2 * margin, cellHeight + 2 * margin);
};

function drawWhisker(p5, cellWidth, cellHeight, n, getTranslatedIndex) {
  p5.background('black');
  p5.blendMode(p5.LIGHTEST);

  for (var expectedIndex = 0; expectedIndex < n; ++expectedIndex) {
    // p5.stroke('tomato');
    // p5.strokeWeight(0.1);
    // p5.line(margin + expectedIndex * cellWidth, margin, margin + expectedIndex * cellWidth, margin + cellHeight);

    var translatedIndex = getTranslatedIndex(expectedIndex);
    if (true || translatedIndex < n) {

      if (translatedIndex === expectedIndex) {
        p5.stroke('orange');
        p5.strokeWeight(0.5);
      }
      else {
        p5.stroke('cyan');
        p5.strokeWeight(1.0);
      }
      p5.line(margin + expectedIndex * cellWidth, margin, margin + translatedIndex * cellWidth, margin + cellHeight);
    }
  }

  p5.noStroke();
};
window.drawWhisker = {
  setup: setupWhisker,
  draw: drawWhisker,
};
```

### The Identity Ordering


```p5js/playable/autoplay/center
var n = 20;
var cellWidth = 10;
var cellHeight = 200;

p5.setup = function() {
  window.drawWhisker.setup(p5, cellWidth, cellHeight, n);
};

function identity(index) {
  return index;
};

p5.draw = function() {
  window.drawWhisker.draw(p5, cellWidth, cellHeight, n, identity);
};
```


### The Non-overlapping Pairwise Swap Ordering

This is the permutation

$$\left((1,2)(3,4)\dots\right)$$

The effect of the `pairwise` function below is to swap pairs of the form $(2n + 1, 2n + 2)$ (an odd number followed by an even number), so that once a pair is swapped, the resulting swapped number is not considered in any further computation.


```p5js/playable/autoplay/center
var n = 100;
var cellWidth = 10;
var cellHeight = 200;

p5.setup = function() {
  window.drawWhisker.setup(p5, cellWidth, cellHeight, n);
};

function pairwise(row) {
  return row % 2 === 0 ? (row + 1) : (row - 1);
};

p5.draw = function() {
  window.drawWhisker.draw(p5, cellWidth, cellHeight, n, pairwise);
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

var n = 100;
var cellWidth = 10;
var cellHeight = 100;

p5.setup = function() {
  window.drawWhisker.setup(p5, cellWidth, cellHeight, n);
};

function pairwise(row) {
  if (!pairwiseCalculated) {
    pairwiseCalculated = new Array(n);
    for (var i = 0; i < n; i += 1) {
      pairwiseCalculated[i] = i + 1;
      pairwiseCalculated[i + 1] = i;
    }
  }

  return pairwiseCalculated[row];
}

p5.draw = function() {
  window.drawWhisker.draw(p5, cellWidth, cellHeight, n, pairwise);
};

```



### The N-wise Swap Ordering

This is a more dynamic version of the pairwise swap. In this case, we'll be doing a pairwise swap for each new element, but we will not affect an element that has already been swapped.

```p5js/playable/autoplay/center

var n = 100;
var cellWidth = 10;
var cellHeight = 400;

p5.setup = function() {
  window.drawWhisker.setup(p5, cellWidth, cellHeight, n);
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
  window.drawWhisker.draw(p5, cellWidth, cellHeight, n, nwise);
};
```



### The Prime-Squared Ordering

We're just going to play around and write an ordering that has algorithmic complexity in time, but is bounded in space. It turns out that this is really tricky for the *fun* orderings, because when we are dealing with the generation of infinite sequences, we need to be cognizant of how much *remembering* our algorithm must do to create a particular ordering.

Just for kicks, we'll map an element $n$ of $\mathrm{N}$ as follows:
- If $n$ is prime, then we will map $n$ to $n^2$.
- If $n$ is the square of a prime $p$, then we will map $n$ to $p$.
- Otherwise, we use the identity mapping.

This should effect a swap of each prime with its square, and uses no memory (except that used by `Math.sqrt` and other mathematical operations that scale non-linearly).

```p5js/playable/autoplay/center

var n = 10;
var cellWidth = 15;
var cellHeight = 400;

p5.setup = function() {
  window.drawWhisker.setup(p5, cellWidth, cellHeight, n);
};


function isPrime(n) {
  const maxDivisor = Math.sqrt(n);
  for (let i = 2; i <= maxDivisor; ++i) {
    if (n % i === 0) {
      return false;
    }
  }
  return true;
}

function mapPrimes(nIndex) {
  let result = nIndex;

  if (isPrime(nIndex + 1)) {
    result = (nIndex + 1) * (nIndex + 1) - 1;
  }
  else {
    const sqrt = Math.sqrt(nIndex + 1);
    if (Math.floor(sqrt) === sqrt) {
      if (isPrime(sqrt)) {
        result = sqrt - 1;
      }
    }
  }
  return result;
}

p5.draw = function() {
  window.drawWhisker.draw(p5, cellWidth, cellHeight, n, mapPrimes);
};
```

These sim

---

[Home](:@Home)

