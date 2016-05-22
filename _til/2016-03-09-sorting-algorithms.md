---
title:  "Sorting Algorithms"
date:   2016-03-09 17:33:00
category: til
tags: [algorithms, teaching]
---

TIL (more like re-learned) the differences between a few basic sorting algorithms: `selection sort`, `insertion sort`, and `merge sort`

This week during class I introduced students to 3 sorting algorithms that could potentially appear on the AP exam: `selection sort`, `insertion sort`, and `merge sort`. Students are NOT required to know how to implement these algorithms (in java) on the exam, but should be familiar with how they work and what a collection will look like after 'n' passes of the algorithm.

### Teaching Techniques

I started the class by reminding students how much *faster* binary search is than straight sequential search, and that binary search only works on a *sorted collection*. This **motivated** the need for efficient sorting algorithms.

I created a **slide deck** that covered 3 main parts for each algorithm:

  1. How the algorithm works (written in english)
  2. How the algorithm looks like after n passes using a small array of 4 elements
  3. How the algorithm performs in best and worst case scenarios

Then, I split the students up in to groups of 3 and gave each group a set of notecards with the numbers 0 - 9 written on them. They shuffled these cards in random order on their desk, and practiced implementing the algorithm. I walked around and corrected any mistakes I saw while they were working.

Finally, I found some Youtube videos (linked in the headers below) that helped illustrate each algorithm in a more...engaging way :)

### [Selection Sort][Selection]{:target="_blank"}

Possibly one of the worst sorting algorithms out there performance-wise, this was a good place to start our discussion.

`Selection sort` works by starting with the element at index 0 of the array, and comparing the element to **every other element in the array** to find the smallest element. The smallest element and the element in position 0 are then swapped, so the smallest element is now in position 0. This algorithm is repeated for each element in the array.

After 'k' passes of the algorithm, the first 'k' elements of the array are in their final sorted position.

The number of comparisons necessary to complete this algorithm is on the order of `O(n^2)`, since each element needs to be compared to every other element once. There is no pre-ordering that makes this algorithm perform any more efficiently.

__Example:__

{%highlight javascript%}
  //original array
  arr = [8, 4, 5, 9, 1, 7, 3, 6, 2];

  //after 1 pass of selection sort
  arr = [1, 4, 5, 9, 8, 7, 3, 6, 2];

  //after 2 passes
  arr = [1, 2, 5, 9, 8, 7, 3, 6, 4];

  //after 3 passes
  arr = [1, 2, 3, 9, 8, 7, 5, 6, 4];

  //and so on...
{% endhighlight %}


**Something incorrect** that students were doing when implementing the algorithm on their own, was to make a swap each time a smaller item was detected during a single pass... **selection sort only makes one swap per pass through the array:** the swap between elements at the starting index, and the index of the smallest item found during the iteration.

### [Insertion Sort][Insertion]{:target="_blank"}

`Insertion sort` works by creating a "sorted portion" of the array at the beginning,  adding elements to it (in sorted order) one by one. This sorted portion starts off as one element (which is sorted relative to itself), then grows to 2 sorted elements, then 3, and so on until the entire array is sorted.

After 'k' passes through the algorithm, the first 'k' elements are sorted relative to each other, not necessarily relative to the rest of the elements in the array.

The number of operations necessary to complete this algorithm is still on the order of `O(n^2),` but there is a particular case when the algorithm performs better than any other sorting algorithm in existence...**and that is when the array is already sorted!** In this situation, Insertion sort performs exactly `n-1` comparisons, since each element is already in its final sorted position and just needs to check the element behind it to confirm this (which is on the order of `O(n)`!)

__Example:__

{%highlight javascript%}
  //original array
  arr = [8, 4, 5, 1, 9, 7, 3, 6, 2];

  //after 1 pass of selection sort
  arr = [4, 8, 5, 1, 9, 7, 3, 6, 2];

  //after 2 passes
  arr = [4, 5, 8, 1, 9, 7, 3, 6, 4];

  //after 3 passes
  arr = [1, 4, 5, 8, 9, 7, 5, 6, 4];

  //and so on...
{% endhighlight %}

### [Merge Sort][Merge]{:target="_blank"}

`Merge sort` works by dividing the starting array in to 2 arrays of (roughly) equal length, then splitting each sub-array recursively until there is one element per sub array. Two adjacent sub-arrays are combined by examining the elements of each sub-array one by one, and placing each item in to the a "merged array" in sorted order.

This is an example of a divide and conquer algorithm, and was originally created by John Von Neumann in 1945. Since it involves similar work being done on separate halves of an array of variable size, it makes sense to implement this algorithm as a recursive method.

The performance of this sorting algorithm is `O(nlog(n))`, with the log(n) portion coming from the number of splits it takes to get an array down to single element sub-arrays. There is no better or worse performance based on the pre-ordering of data. Merge sort requires the creation of an additional array of length `startingArray.length` in which the sorted version will be constructed, so **merge sort is more space inefficient** than selection or insertion sort.

{%highlight javascript%}
 //The following shows how the left-most elements are split in to sub-arrays
 //of length 1 to be merged back together in sorted order.
 startingArr = [4, 2, 3, 1];
 subArr1 = [4, 2];
 subArr3 = [4]; subArr4 = [2];

 //Once splitting is complete, sub-arrays are combined in to a merged
 //array that is itself in sorted order.
 mergedArr1 = [2, 4];

 //Next the right half of the array is split.
 subArr2 = [3,1];
 subArr5 = [3], subArr6 = [1]

 //Then, it is combined in to a sorted merged array.
 mergedArr2 = [1,3];

 //Finally, elements are combined from the two merged arrays in to an array
 //of `startingArr.length`, which is returned as the final sorted array.

 finalArr = [1, 2, 3, 4]

 //Note that items are added to finalArr in the following order:
 //mergedArr2[0], mergedArr1[0], mergedArr2[1], mergedArr1[1]
{%endhighlight%}

Students were able to recognize that merge sort should be implemented using recursion, which was awesome!

I think the videos + notecard shuffling was helpful to understand the algorithms with some more hands on activities. We'll see how well they know their stuff on the unit test this week!

*NOTE: All credit for videos linked in this blog post goes to the creators, whose information can be found in the video descriptions on youtube.*

[Selection]: https://www.youtube.com/watch?v=LriMvv9qDrk
[Insertion]: https://www.youtube.com/watch?v=ROalU379l3U
[Merge]: https://www.youtube.com/watch?v=XaqR3G_NVoo


