---
layout: submission
title: "Submission by hyszeth.eth for challenge Unique"
submitter: "hyszeth.eth"
wild: False
challenge: Unique
gas: 644838
submitted: 2018-06-22 07:34:36.631102+00:00
---
```solidity
/**
 * This file is part of the 1st Solidity Gas Golfing Contest.
 *
 * This work is licensed under Creative Commons Attribution ShareAlike 3.0.
 * https://creativecommons.org/licenses/by-sa/3.0/
 */

pragma solidity 0.4.24;

contract Unique {
    event E(uint);
    /**
     * @dev Removes all but the first occurrence of each element from a list of
     *      integers, preserving the order of original elements, and returns the list.
     *
     * The input list may be of any length.
     *
     * @param input The list of integers to uniquify.
     * @return The input list, with any duplicate elements removed.
     */


    function uniquify(uint[] input) public pure returns(uint[] ret) {
        if(input.length == 0) return ret;
        uint inputLength = input.length;
        uint lookupLen = 1051;
        bool[1051] memory lookup;// = new uint[][](lookupLen);
        uint[1051] memory last;

        uint[] memory uniqueList = new uint[](input.length);
        uint uniqueIndex;

        uint nDups;
        uint r1;
        for(uint i = 0; i < input.length; ++i) {
            uint current = input[i];
            uint quickHash = current % lookupLen;
            if(lookup[quickHash]) {
                // Scan unique list for duplicates
                if(last[quickHash] != current) {
                    for(uint j = 0; j < uniqueIndex; ++j) {
                        if(current == uniqueList[j]) break;
                    }
                    if(j == uniqueIndex) {
                        // false positive
                        uniqueList[uniqueIndex++] = current;
                        last[quickHash] = current;
                        r1++;
                    }
                }
            } else {
                uniqueList[uniqueIndex++] = current;
                lookup[quickHash] = true;
                last[quickHash] = current;
            }
        }
        //emit E(r1);
        ret = new uint[](uniqueIndex);
        for(i = 0; i < uniqueIndex; ++i) {
            ret[i] = uniqueList[i];
        }
        return ret;
    }
}

```
