---
layout: submission
title: "Submission by hyszeth.eth for challenge Sort"
submitter: "hyszeth.eth"
wild: False
challenge: Sort
gas: 728666
submitted: 2018-06-26 06:31:13.417722+00:00
---
```solidity
/**
 * This file is part of the 1st Solidity Gas Golfing Contest.
 *
 * This work is licensed under Creative Commons Attribution ShareAlike 3.0.
 * https://creativecommons.org/licenses/by-sa/3.0/
 *
 * Author: Greg Hysen (@hysz)
 * Date: June, 2018
 */

pragma solidity 0.4.24;

contract Sort {

    /**
     * @dev Sorts a list of integers in ascending order.
     *
     * The input list may be of any length.
     *
     * @param input The list of integers to sort.
     * @return The sorted list.
     */
     function sort(uint[] input)
      public
      pure
      returns(uint[])
    {
        if(input.length == 0) return input;
        quickSort(input, 0, input.length - 1);
        return input;
     }

        function quickSort(
            uint[] input,
            uint lo,
            uint hi
        )
         internal
         pure
        {
            uint iElem;
            uint jElem;
            if(hi - lo <= 7) {
                // If within threshold, run insertion sort instead.
                i = lo + 1;
                while(i <= hi) {
                    jElem=input[(j = i)];
                    while((iElem=input[j-1]) > jElem) {
                        input[j--] = iElem;
                        if(j == lo) break;
                    }
                    if(j != i++) input[j] = jElem;
                }
                return;
            }

            // Pivot (Median of 3)
            i = uint((hi+lo)/2); // pivot index
            iElem = input[lo];
            uint pivot = input[i];
            jElem = input[hi];
            // [iElem, pivot, jElem]
            if(iElem >= pivot) { // [pivot, iElem] [jElem]
                if(iElem <= jElem) {
                    // [pivot, iElem, jElem]
                    // Only swap pivot and iElem
                    input[lo] = pivot;
                    input[i] = iElem;
                    pivot = iElem;
                } else {
                    // [pivot, jElem, iElem] or [jElem, pivot, iElem]
                    if(jElem >= pivot) {
                        // [pivot, jElem, iElem]
                        input[lo] = pivot;
                        input[i] = jElem;
                        input[hi] = iElem;
                        pivot = jElem;
                    } else {
                        // [jElem, pivot, iElem]
                        input[lo] = jElem;
                        input[hi] = iElem;
                    }
                }
            } else {// if(iElem < pivot) { // [iElem, pivot] [jElem]
                if(pivot >= jElem) {
                    // [iElem, jElem, pivot] or [jElem, iElem, pivot]
                    if(jElem >= iElem) {
                        // [iElem, jElem, pivot]
                        input[i] = jElem;
                        input[hi] = pivot;
                        pivot = jElem;
                    } else {
                        // [jElem, iElem, pivot]
                        input[lo] = jElem;
                        input[i] =  iElem;
                        input[hi] = pivot;
                        pivot = iElem;
                    }
                } else {
                    // [iElem, pivot, jElem]
                    // Do nothing
                }
            }

            // Hoare pivot algorithm
            // Note we skip the lo/hi elements because
            // we already know they're in the correct position.
            uint j = hi;
            uint i = lo;
            if(iElem != jElem) {
                // We're going to unroll the loop here.
                // Disgusting? Yes. But, it saved ~5k gas :').
                while(true) {
                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;


                    while((iElem=input[uint(++i)]) <= pivot) {}
                    while((jElem=input[uint(--j)]) >= pivot) {}
                    if(i >= j) {
                        // Sort
                        quickSort(input, lo, j);
                        return quickSort(input, i, hi);
                    }
                    input[uint(i)] = jElem;
                    input[uint(j)] = iElem;
                }
                return;
            }

            // Fallback in case we can't use `<=` and `>=`
            // This version is much less efficient.
            while(true) {
                while((iElem=input[uint(++i)]) < pivot) {}
                while((jElem=input[uint(--j)]) > pivot) {}

                if(i >= j) {
                    // Sort
                    quickSort(input, lo, j);
                    return quickSort(input, i, hi);
                }
                input[uint(i)] = jElem;
                input[uint(j)] = iElem;
            }
        }
}

```
