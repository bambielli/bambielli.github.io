---
title:  "Linear Algebra Review"
date:   2017-05-28 23:01:00
category: til
tags: [math, linear-algebra, python, udacity, georgia-tech]
---

TIL how to calculate the magnitude, normalization, dot product, and cross product of vectors. I wrote a small library that performs the calculations - repo within!

Find the [vector library I created here][vector]{:target="_blank"}, which contains implementations of all of the examples discussed below. Library is written in python... wow did I miss list comprehension!

I was recently accepted to the [OMSCS program through Georgia Tech][GT]{:target="_blank"} where I plan to extend my CS knowledge with machine learning and AI focused coursework. I'm starting in August, but as I was looking through the course catalog I came to realize that I'm a bit rusty in some of the core math domains that will be critical for my coursework...

Fortunately, OMSCS offers a [linear algebra refresher course][linear]{:target="_blank"} on Udacity for students in the same boat as myself! The first lesson goes over basic vector algebra and operations on pairs of vectors. Here's what I learned:

### Vectors and Basic Operations

A Vector is a geometric construct that has both a **direction** and a **length** (also referred to as a *magnitude*). Basic arithmetic operations like addition, subtraction, and multiplication are possible on vectors. We often think of vectors as lines in 2D or 3D space, but N dimensional vectors are also permitted.

To **add** two vectors, just sum their coordinates together! e.g. the vector [1, 2] and [2, 3] sum to the vector [3, 5].

<figure>
  <img src="/assets/images/Add.jpg">
  <figcaption>Geometric representation of the sum of vectors U and V</figcaption>
</figure>

**Subtracting** two vectors follows the same process, except the process isn't commutative like addition is. e.g. the vector [1, 2] minus [1, 1] equals the vector [0, 1].

<figure>
  <img src="/assets/images/Subtract.jpg">
  <figcaption>Geometric representation of U - V</figcaption>
</figure>

**Multiplying** a vector by a scalar is also possible by simply multiplying each dimension of the vector by the scalar. e.g. the vector [1, 2] scaled by 2 equals the vector [2, 4].

<figure>
  <img src="/assets/images/Multiply.jpg">
  <figcaption>Geometric representation of scaling the vector U by 2</figcaption>
</figure>

It is also possible to multiply by non integer scalars, and negative scalars.

<figure>
  <img src="/assets/images/MultiplyNegative.jpg">
  <figcaption>Geometric representation of scaling the vector U by -0.5</figcaption>
</figure>

### Magnitude, Unit Vector, and Normalization

The `magnitude` of the vector is another word for the length of the vector. It represents the distance between the head and tail of a vector. If we think of a vector as the hypotenuse of a triangle, we can use pythagorean's theorem to calculate its length by taking the square root of the sum of squares of its coordinates.

`Mag(V) = sqrt(V1^2 + V2^2 + ... + Vn^2)`

Where `V1, V2 ... Vn` are the values of each dimension of vector V.


A `unit vector` is a vector in any direction with magnitude of 1. The vector [1, 1] is an example of a unit vector, since the square root of the sum of squares of this vector is 1. It is not, however, the only unit vector that exists... in fact, every vector has an equivalent unit vector! The process of finding a unit vector *in the same direction* as a given vector is called `normalization`. The normalization formula is as follows:

`Norm(V) = (1 / Mag(V)) * V`

Where V is the vector we hope to normalize.


### The Dot Product

Another operation that is possible to perform on a pair of vectors is the `dot product`, also known as the `scalar product` or `inner product`. The dot product is a scalar representation of two vectors. It also can be used to describe the angle between two vectors geometrically (the acute angle between the pair, not the obtuse angle). The geometric definition of the dot product is as follows:

`V . W = Mag(V) * Mag(W) * cos(theta)`

Where V and W are vectors and theta is the angle between them.

Manipulating the formula above allows us to determine the angle between two vectors. The scalar value produced by the dot product is also used in other types of vector calculations that we will see later.

If we do not know the angle between the vectors, we can calculate the dot product using the algebraic definition:

`V . W = V1 * W1 + V2 * W2 + ... + Vn * Wn`

Where V and W are vectors and `Vn` and `Wn` are the values at the nth dimension of vectors V and W.

Some rules that fall out of this theorem: if `V . W` is 0, and V and W are non-zero, it follows that `cos(theta)` must be 0! **This means that the vectors must be orthogonal to each other.**

### Cross Product

The `cross product` of two vectors is a vector that is orthogonal to both given vectors. Unlike the dot product, the cross product is anti-commutative: `V x W` does not yield the same result as `W x V`. Another constraint, is that **the cross product may only be taken on vectors in 3D space.**

We use the old "right hand rule" to determine the direction of the resulting vector: point your thumb in the direction of V, your index finger in the direction of W, and stick your middle finger perpendicular to both. Your middle finger represents the direction of the resulting vector from the cross product.

Like the dot product, cross product can be calculated in a few different ways. The geometric definition is very similar to the dot product:

`V x W = Mag(V) * Mag(W) * sin(theta)`

The only difference in the equation is that `sin(theta)` is used to calculate the cross product.

If the angle between the vectors is unknown, we can use the algebraic definition to calculate the cross product:

`V x W = [y1\*z1 - y2\*z2, -(x1\*z2 - x2\*z1), x1\*y2 - x2\*y1]`

### When are These Useful?

I tried poking around the internet to try and find some applications of these vector operations. I know they are used in E&M Physics to calculate magnetic flux (dot product) and the direction and magnitude of an electric field (cross product). I'm sure I'll run in to other uses as I continue the refresher!

I also learned about how to calculate the projection of a vector onto a basis vector, and how to find the orthogonal component of a vector when given a basis vector. Check out the [vector library I wrote][vector]{:target="_blank"} to see how those are calculated, along with implementations of the other vector operations I discussed above.

[vector]: https://github.com/bambielli/linearAlgebra/blob/master/unit1/vector.py
[GT]: https://www.omscs.gatech.edu/
[linear]: https://classroom.udacity.com/courses/ud953