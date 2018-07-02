---
layout: submission
title: "Submission by hyszeth.eth for challenge BrainFuck"
submitter: "hyszeth.eth"
wild: False
challenge: BrainFuck
gas: 5429343
submitted: 2018-06-07 09:17:16.203325+00:00
---
```solidity
/**
 * This file is part of the 1st Solidity Gas Golfing Contest.
 *
 * This work is licensed under Creative Commons Attribution ShareAlike 3.0.
 * https://creativecommons.org/licenses/by-sa/3.0/
 */

pragma solidity 0.4.24;

contract BrainFuck {
    /**
     * @dev Executes a BrainFuck program, as described at https://en.wikipedia.org/wiki/Brainfuck.
     *
     * Memory cells, input, and output values are all expected to be 8 bit
     * integers. Incrementing past 255 should overflow to 0, and decrementing
     * below 0 should overflow to 255.
     *
     * Programs and input streams may be of any length. The memory tape starts
     * at cell 0, and will never be moved below 0 or above 1023. No program will
     * output more than 1024 values.
     *
     * @param program The BrainFuck program.
     * @param input The program's input stream.
     * @return The program's output stream. Should be exactly the length of the
     *          number of outputs produced by the program.
     */
    function execute(bytes program, bytes input) public pure returns(bytes) {
        bytes memory data = new bytes(2048); // store output at end of data
        uint oPtr = runProgram(program, input, data, 1024, 0, 0, 0, program.length);
        uint j;
        bytes memory output = new bytes(oPtr - 1024);
        for(uint i = 1024; i < oPtr; ++i) {
            output[j++] = data[i];
        }
        return output;
    }

    function runProgram(bytes program, bytes input, bytes data, uint oPtr, uint pPtr, uint iPtr, uint dPtr, uint maxProgramLength) internal pure returns(uint) {
        uint[] memory programBegin = new uint[](1024); // max stack depth
        uint programBeginIndex;
        byte s;
        uint n;
        for(;pPtr < maxProgramLength;) {
            if((s=program[pPtr]) == '[') {
                programBegin[programBeginIndex++] = ++pPtr;
                continue;
            } else if(s == ']') {
                if(data[dPtr] == 0) {
                    programBeginIndex--;
                } else {
                    pPtr = programBegin[programBeginIndex-1];
                    continue;
                }
            } else if(s == '>') {
                dPtr++;
            } else if(s == '<') {
                dPtr--;
            } else if(s == '+') {
                data[dPtr] = byte(uint8(data[dPtr]) + 1);
            } else if(s == '-') {
                data[dPtr] = byte(uint8(data[dPtr]) - 1);
            } else if(s == ',') {
                data[dPtr] = input[iPtr++];
            } else if(s == '.') {
                data[oPtr++] = data[dPtr];
            }
            pPtr++;
        }

        return oPtr;
    }
}

```
