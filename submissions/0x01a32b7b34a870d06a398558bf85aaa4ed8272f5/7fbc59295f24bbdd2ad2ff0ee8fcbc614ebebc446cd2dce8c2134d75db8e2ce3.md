---
layout: submission
title: "Submission by 0x01a32b7b34a870d06a398558bf85aaa4ed8272f5 for challenge HexDecoder"
submitter: "0x01a32b7b34a870d06a398558bf85aaa4ed8272f5"
wild: False
challenge: HexDecoder
gas: 2832677
submitted: 2018-06-16 18:43:57.474803+00:00
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
    /**
     * @dev Decodes a hex-encoded input string, returning it in binary.
     *
     * Input strings may be of any length, but will always be a multiple of two
     * bytes long, and will not contain any non-hexadecimal characters.
     *
     * @param _input The hex-encoded input.
     * @return The decoded output.
     */
    function decode(string _input) public pure returns(bytes output) {
      bytes memory input = bytes(_input);
      output = new bytes(input.length / 2);

      bytes1 n1;
      bytes1 n2;
      for (uint i = 0; i < output.length; i++) {
        if (int8(input[i << 1]) < 58) {
          n1 = bytes1(int8(input[i*2]) - 48);
        } else if (int8(input[i*2]) < 71) {
          n1 = bytes1(int8(input[i*2]) - 55);
        } else {
          n1 = bytes1(int8(input[i*2]) - 87);
        }

        if (int8(input[i*2+1]) < 58) {
          n2 = bytes1(int8(input[i*2+1]) - 48);
        } else if (int8(input[i*2+1]) < 71) {
          n2 = bytes1(int8(input[i*2+1]) - 55);
        } else {
          n2 = bytes1(int8(input[i*2+1]) - 87);
        }

        output[i] = (n1 << 4) | n2;
      }

      return output;
    }
}


```
