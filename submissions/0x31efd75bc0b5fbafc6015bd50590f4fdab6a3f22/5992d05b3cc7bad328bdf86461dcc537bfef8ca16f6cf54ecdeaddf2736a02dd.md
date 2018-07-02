---
layout: submission
title: "Submission by 0x31efd75bc0b5fbafc6015bd50590f4fdab6a3f22 for challenge HexDecoder"
submitter: "0x31efd75bc0b5fbafc6015bd50590f4fdab6a3f22"
wild: False
challenge: HexDecoder
gas: 2900991
submitted: 2018-06-08 16:55:00.285007+00:00
---
```solidity
/**
 * This file is part of the 1st Solidity Gas Golfing Contest.
 *
 * This work is licensed under Creative Commons Attribution ShareAlike 3.0.
 * https://creativecommons.org/licenses/by-sa/3.0/
 */

pragma solidity 0.4.24;

contract HexDecoder {

    function mkb(uint len) internal pure returns (bytes b) {
        b = new bytes(len);
    }

    /**
     * @dev Decodes a hex-encoded input string, returning it in binary.
     *
     * Input strings may be of any length, but will always be a multiple of two
     * bytes long, and will not contain any non-hexadecimal characters.
     *
     * @param input The hex-encoded input.
     * @return The decoded output.
     */
    function decode(string input) public pure returns(bytes output) {
       uint len = bytes(input).length;
        bytes memory res = mkb(len/2);

        uint total;
        for (uint i = 0; i < bytes(input).length; i++) {
            total <<= 4;
            byte c = bytes(input)[i];
            if ((c > 0x2f)&&(c < 0x3a)) 
                total += uint(c) - 0x30;
            else if ((c > 0x40)&&(c < 0x47)) 
                total += uint(c) - uint(0x37);
            else
                total += uint(c) - uint(0x57);
            if ((i % 2) == 1)
                res[i/2] = byte(total);
        }
        output = res;
    }
}

```
