---
layout: submission
title: "Submission by 0x53b21be84eece2e615016c337edcda204758704b for challenge Sort"
submitter: "0x53b21be84eece2e615016c337edcda204758704b"
wild: False
challenge: Sort
gas: 744429
submitted: 2018-05-28 16:29:56.455855+00:00
---
```solidity
/**
 * This file is part of the 1st Solidity Gas Golfing Contest.
 *
 * This work is licensed under Creative Commons Attribution ShareAlike 3.0.
 * https://creativecommons.org/licenses/by-sa/3.0/
 */
pragma solidity ^0.4.24;

// gas used 1 ,     1173 ,   per_uint 1173
// gas used 1 ,     638928 ,   per_uint 638928
// gas used 1 ,     151751 ,   per_uint 151751
// gas used 1 ,     151751 ,   per_uint 151751
// Total gas for Sort: 943603

contract Sort {
    function sort(uint[] input) public pure returns(uint[]) {
        uint len = input.length;
        if (len == 0) return input;
        
        uint bucketsize = len > 10 ? len : 10;
        uint[] memory list = new uint[](len);
        uint[] memory ans = new uint[](len);
        uint[] memory tmp;

        uint maxbucket = len < 200 ? 1 : bucketsize;
        uint bucketmult = 1;
        uint i;

        while (bucketmult <= maxbucket) {
            uint[] memory buckets = new uint[](bucketsize);

            for (i = 0; i < len; i++) {
                uint bucket = (input[i] / bucketmult) % bucketsize;
                list[i] = buckets[bucket];
                buckets[bucket] = i + 1;
            }

            uint j = len;
            for (i = bucketsize; --i != 0;) {
                for (bucket = buckets[i]; bucket != 0; bucket = list[bucket - 1]) {
                    ans[--j] = input[bucket - 1];
                }
            }
            tmp = input;
            input = ans;
            ans = tmp;
            bucketmult *= bucketsize;
        }


        return input;
    }
}
```
