---
layout: submission
title: "Submission by ricmoo.firefly.eth for challenge BrainFuck"
submitter: "ricmoo.firefly.eth"
wild: True
challenge: BrainFuck
gas: 386093
submitted: 2018-06-06 04:32:09.178105+00:00
---
```solidity
pragma solidity 0.4.24;

contract BrainFuck {
    function execute(bytes /* program */, bytes /* input */) public pure returns(bytes) {
        assembly {

            // Memory Layout
            //  -    0:  output pointer (32 bytes)
            //  -   32:  output length (32 bytes)
            //  -   64:  output buffer (1024 bytes)
            //  - 1088:  memory buffer (1024 bytes)
            //  - 2112:  transcoded BFI source; 16-byte wide instructions
            //
            // Note: temporarily uses [0..256] during transcaoding to store a LUT

            // Free-memory pointer
            let output := mload(0x40)

            // Transcoded BrainFuck Intermediate (BFI) Format
            //  - Jump:        0x0000 | jumpDest   (14 bits)
            //  - JumpNot:     0x8000 | jumpDest   (14 bits)
            //  - Math:        0xc000 | adjustment ( 8 bits; signed)
            //  - Move:        0x4000 | adjustment ( 8 bits; signed)
            //  - I/O (read):  0x0600
            //  - I/O (write): 0x0700
            //
            // Notes:
            //    - for non-jump operations top 3 MSB indicate operation
            //    - for jumps, top 1 MSB indicate negate operation

            let code := add(output, 2112)

            let op := 0
            let eof := code
            let pc0 := add(calldataload(4), 4)

            {
                // Lookup Table (we hijack the output for this step)
                //  - "[" = 0x80 (jump forward class = 0x80)
                //  - "]" = 0xc0 (jump back class    = 0xc0)
                //  - "+" = 0x22 (adjustment class   = 0x20)
                //  - "-" = 0x20 (adjustment class   = 0x20)
                //  - "<" = 0x30 (adjustment class   = 0x20)
                //  - ">" = 0x32 (adjustment class   = 0x20)
                //  - "," = 0x60 (I/O class          = 0x60)
                //  - "." = 0x70 (I/O class          = 0x60)
                //
                // Notes:
                //  - Top 3 MSB indicate operation class
                //  - For adjustment class, bottom 2 LSB indicates increase (0x02) or decrease (0x00)

                //mstore(add(output, 32), 0x0000000000000000000000226020700000000000000000000000000030003200)
                //mstore(add(output, 64), 0x0000000000000000000000000000000000000000000000000000008000c20000)
                let lut := 0x30003200000000000000000000220020600070000000000000000000008000c2

                let lastOp := 0
                let lastOpen := 0
                let accum := 0
                let end := add(add(pc0, calldataload(pc0)), 32)
                //let end := calldataload(pc0)
                pc0 := add(pc0, 32)
                //end := add(pc, end)
/*
            mstore(output, 32)
            mstore(add(output, 32), 96)
            mstore(add(output, 64), pc0)
            mstore(add(output, 96), end)
            mstore(add(output, 128), calldataload(pc0))
            return(output, 160)
*/
                let chunk := 0
                for { } lt(pc0, end) { } {
                    //op := byte(0, mload(add(output, byte(0, calldataload(pc0)))))
                    //op := byte(0, mload(add(output, byte(0, calldataload(pc0)))))
                    let chunkIndex := and(sub(pc0, 4), 0x1f)
                    if iszero(chunkIndex) {
                        chunk := xor(calldataload(pc0), 0x1111111111111111111111111111111111111111111111111111111111111111)
                    }
                    //op := byte(sub(xor(17, byte(0, calldataload(pc0))), 45), lut)
                    op := byte(sub(byte(chunkIndex, chunk), 45), lut)
                    switch and(op, 0xe0)
                        case 0x00 {  // Junk
                        }
                        case 0x20 {  // "<", ">", "+" and "-"
                            if iszero(eq(lastOp, and(0x30, op))) {
                                lastOp := and(0x30, op)
                                mstore8(eof, lastOp)
                                accum := 0
                                eof := add(eof, 2)
                            }
                            accum := add(accum, sub(and(op, 0x02), 1))
                            mstore8(sub(eof, 1), accum)
                        }
                        case 0x80 { // "["
                            mstore8(eof, div(lastOpen, 256))
                            mstore8(add(eof, 1), lastOpen)
                            lastOpen := eof
                            eof := add(eof, 2)
                            lastOp := 0
                        }
                        case 0xc0 { // "]"
                            // Temporarily use accum as scratch; it isn't valid after a "[" anyways
                            accum := div(mload(lastOpen), 0x1000000000000000000000000000000000000000000000000000000000000)
                            // @TODO: optimize target - 30
                            mstore8(lastOpen, div(sub(eof, 30), 256))
                            mstore8(add(lastOpen, 1), sub(eof, 30))
                            mstore8(eof, or(0x80, div(sub(lastOpen, 30), 256)))
                            mstore8(add(eof, 1), sub(lastOpen, 30))
                            lastOpen := accum
                            eof := add(eof, 2)
                            lastOp := 0
                        }
                        case 0x60 { // "," and "."
                            mstore8(eof, op)
                            eof := add(eof, 2)
                            lastOp := 0
                        }
                    pc0 := add(pc0, 1)
                }
            }
/*
            // Dump out BFI code
            mstore(sub(code, 66), 32)
            mstore(sub(code, 34), add(sub(eof, code), 2))
            mstore8(sub(code, 2), div(code, 256))
            mstore8(sub(code, 1), code)
            return (sub(code, 66), add(sub(eof, code), 66))
*/
            // Make sure we clear out the LUT from above
            mstore(add(output, 64), 0x0)

            // Beginning of actual output (after dynlink0 and bytes length)
            let outputIndex := add(output, 64)
            {
                // Put data pointer at beginning of memory
                let dp := add(output, 1088)

                // Beginning of calldata input (sig + dynlink0; skip length)
                //let inputIndex := add(calldataload(36), 36)
                let inputIndex := add(calldataload(36), 5)

                // For pc0 = code to eof...
                pc0 := sub(code, 30)
                eof := sub(eof, 30)
                for { } lt(pc0, eof) { } {
                    op := mload(pc0)
                    switch and(op, 0xf000)
                        case 0x2000 {  // Math
                            //mstore8(dp, add(byte(0, mload(dp)), signextend(0, and(op, 0xff))))
                            //mstore8(dp, add(byte(0, mload(dp)), and(op, 0xff)))
                            mstore8(dp, add(mload(sub(dp, 31)), op))
                        }
                        case 0x3000 {  // Move
                            dp := add(dp, signextend(0, and(op, 0xff)))
                        }
                        case 0x6000 {  // Input
                            mstore8(dp, calldataload(inputIndex))
                            //mstore8(dp, calldataload(sub(inputIndex, 31)))
                            //mstore(dp, calldataload(inputIndex))
                            inputIndex := add(inputIndex, 1)
                        }
                        case 0x7000 {  // Output
                            mstore8(outputIndex, byte(0, mload(dp)))
                            outputIndex := add(outputIndex, 1)
                        }
                        default {    // Jump
                            //op := div(op, 0x1000000000000000000000000000000000000000000000000000000000000)
                            if eq(iszero(and(op, 0x8000)), iszero(byte(0, mload(dp)))) {
                                pc0 := and(0x7fff, op)
                            }
                        }
                    pc0 := add(pc0, 2)
                }
            }

            // Store dynlink0 pointing to 0x20
            mstore(output, 32)

            // Store the length (less the dynlink0 and length)
            let outputLength := sub(outputIndex, output)
            mstore(add(output, 32), sub(outputLength, 64))

            // Round length to nearest 32 byte block
            outputLength := and(add(outputLength, 31), 0xffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe0)

            return(output, outputLength)
        }
    }
}

```
