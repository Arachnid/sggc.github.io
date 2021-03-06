---
layout: submission
title: "Submission by wicketh.eth for challenge Sort"
submitter: "wicketh.eth"
wild: True
challenge: Sort
gas: 179871
submitted: 2018-06-28 16:19:30.495337+00:00
---
```solidity
pragma solidity ^0.4.23;

contract Sort {
    
    function () external payable { assembly {
    
    // function sort(uint[] input) external payable returns(uint[]) { assembly {
        
        // @author Remco Bloemen <remco@wicked.ventures>
        
        // No  unrepeated sorted input
        // Yes repeated sorted input
        // No  unrepeated reverse sorted input
        // Yes repeated reverse sorted input
        
        mstore(0x40, 0) // Set all memory to zero
        
        let temp1
        let temp2
        let addr1
        let addr2
        let scale
        let i
        
        // Special case for zero or one value
        jumpi(trivial, lt(calldatasize, 0x84))
        
        // Radix sort
        // Size of bucket table:
        //                                      Competition
        //  60 * 32 = 1920    gas: 337644        208971
        //  80 * 32 = 2560    gas: 323985        194330
        // 100 * 32 = 3200    gas: 321639        193251
        // 110 * 32 = 3520    gas: 318570        190720
        // 120 * 32 = 3840    gas: 315349        186302
        // 130 * 32 = 4160    gas: 314771        188363
        // 140 * 32 = 4480    gas: 314657        186329
        // 160 * 32 = 5120    gas: 317469        189141
        // 256 *  1 = 256
        
        // First pass (input):
        // * find upper bound to values
        // * check for sorted input
        // * check for reverse sorted input
        temp1 := calldataload(0x44)
        scale := temp1
        i := 0x64
        addr1 := 1
        addr2 := 1
    l1: // [temp1 addr1 addr2 scale i]
        temp2 := calldataload(i)
        addr1 := and(addr1, slt(sub(temp1, 1), temp2))
        addr2 := and(addr2, gt(add(temp1, 1), temp2))
        scale := or(scale, temp2)
        temp1 := temp2
        i := add(i, 32)
        jumpi(l1, lt(i, calldatasize))
        jumpi(trivial, addr1)
        jumpi(reverse, addr2)
        
        // max(input size) = 299
        
        // Compute scaling factor
        scale := div(add(scale, 119), 120)
        
        // Second pass (input): count buckets (in multipes of 32)
        0x44
    l2: // Stack: i
        // temp1 := mul(div(calldataload(i), scale), 32)
        dup1 scale swap1 calldataload div 32 mul
        // mstore(temp1, add(mload(temp1), 32))
        dup1 mload 32 add swap1 mstore
        // i := add(i, 32)
        32 add
        // jumpi(l2, lt(i, calldatasize))
        dup1 calldatasize gt l2 jumpi
        pop
        
        // Third pass (buckets): running total in buckets
        3840 // Start with write offset
        0x0 mload add dup1 0x0 mstore
        0x20 mload add dup1 0x20 mstore
        0x40 mload add dup1 0x40 mstore
        0x60 mload add dup1 0x60 mstore
        0x80 mload add dup1 0x80 mstore
        0xa0 mload add dup1 0xa0 mstore
        0xc0 mload add dup1 0xc0 mstore
        0xe0 mload add dup1 0xe0 mstore
        0x100 mload add dup1 0x100 mstore
        0x120 mload add dup1 0x120 mstore
        0x140 mload add dup1 0x140 mstore
        0x160 mload add dup1 0x160 mstore
        0x180 mload add dup1 0x180 mstore
        0x1a0 mload add dup1 0x1a0 mstore
        0x1c0 mload add dup1 0x1c0 mstore
        0x1e0 mload add dup1 0x1e0 mstore
        0x200 mload add dup1 0x200 mstore
        0x220 mload add dup1 0x220 mstore
        0x240 mload add dup1 0x240 mstore
        0x260 mload add dup1 0x260 mstore
        0x280 mload add dup1 0x280 mstore
        0x2a0 mload add dup1 0x2a0 mstore
        0x2c0 mload add dup1 0x2c0 mstore
        0x2e0 mload add dup1 0x2e0 mstore
        0x300 mload add dup1 0x300 mstore
        0x320 mload add dup1 0x320 mstore
        0x340 mload add dup1 0x340 mstore
        0x360 mload add dup1 0x360 mstore
        0x380 mload add dup1 0x380 mstore
        0x3a0 mload add dup1 0x3a0 mstore
        0x3c0 mload add dup1 0x3c0 mstore
        0x3e0 mload add dup1 0x3e0 mstore
        0x400 mload add dup1 0x400 mstore
        0x420 mload add dup1 0x420 mstore
        0x440 mload add dup1 0x440 mstore
        0x460 mload add dup1 0x460 mstore
        0x480 mload add dup1 0x480 mstore
        0x4a0 mload add dup1 0x4a0 mstore
        0x4c0 mload add dup1 0x4c0 mstore
        0x4e0 mload add dup1 0x4e0 mstore
        0x500 mload add dup1 0x500 mstore
        0x520 mload add dup1 0x520 mstore
        0x540 mload add dup1 0x540 mstore
        0x560 mload add dup1 0x560 mstore
        0x580 mload add dup1 0x580 mstore
        0x5a0 mload add dup1 0x5a0 mstore
        0x5c0 mload add dup1 0x5c0 mstore
        0x5e0 mload add dup1 0x5e0 mstore
        0x600 mload add dup1 0x600 mstore
        0x620 mload add dup1 0x620 mstore
        0x640 mload add dup1 0x640 mstore
        0x660 mload add dup1 0x660 mstore
        0x680 mload add dup1 0x680 mstore
        0x6a0 mload add dup1 0x6a0 mstore
        0x6c0 mload add dup1 0x6c0 mstore
        0x6e0 mload add dup1 0x6e0 mstore
        0x700 mload add dup1 0x700 mstore
        0x720 mload add dup1 0x720 mstore
        0x740 mload add dup1 0x740 mstore
        0x760 mload add dup1 0x760 mstore
        0x780 mload add dup1 0x780 mstore
        0x7a0 mload add dup1 0x7a0 mstore
        0x7c0 mload add dup1 0x7c0 mstore
        0x7e0 mload add dup1 0x7e0 mstore
        0x800 mload add dup1 0x800 mstore
        0x820 mload add dup1 0x820 mstore
        0x840 mload add dup1 0x840 mstore
        0x860 mload add dup1 0x860 mstore
        0x880 mload add dup1 0x880 mstore
        0x8a0 mload add dup1 0x8a0 mstore
        0x8c0 mload add dup1 0x8c0 mstore
        0x8e0 mload add dup1 0x8e0 mstore
        0x900 mload add dup1 0x900 mstore
        0x920 mload add dup1 0x920 mstore
        0x940 mload add dup1 0x940 mstore
        0x960 mload add dup1 0x960 mstore
        0x980 mload add dup1 0x980 mstore
        0x9a0 mload add dup1 0x9a0 mstore
        0x9c0 mload add dup1 0x9c0 mstore
        0x9e0 mload add dup1 0x9e0 mstore
        0xa00 mload add dup1 0xa00 mstore
        0xa20 mload add dup1 0xa20 mstore
        0xa40 mload add dup1 0xa40 mstore
        0xa60 mload add dup1 0xa60 mstore
        0xa80 mload add dup1 0xa80 mstore
        0xaa0 mload add dup1 0xaa0 mstore
        0xac0 mload add dup1 0xac0 mstore
        0xae0 mload add dup1 0xae0 mstore
        0xb00 mload add dup1 0xb00 mstore
        0xb20 mload add dup1 0xb20 mstore
        0xb40 mload add dup1 0xb40 mstore
        0xb60 mload add dup1 0xb60 mstore
        0xb80 mload add dup1 0xb80 mstore
        0xba0 mload add dup1 0xba0 mstore
        0xbc0 mload add dup1 0xbc0 mstore
        0xbe0 mload add dup1 0xbe0 mstore
        0xc00 mload add dup1 0xc00 mstore
        0xc20 mload add dup1 0xc20 mstore
        0xc40 mload add dup1 0xc40 mstore
        0xc60 mload add dup1 0xc60 mstore
        0xc80 mload add dup1 0xc80 mstore
        0xca0 mload add dup1 0xca0 mstore
        0xcc0 mload add dup1 0xcc0 mstore
        0xce0 mload add dup1 0xce0 mstore
        0xd00 mload add dup1 0xd00 mstore
        0xd20 mload add dup1 0xd20 mstore
        0xd40 mload add dup1 0xd40 mstore
        0xd60 mload add dup1 0xd60 mstore
        0xd80 mload add dup1 0xd80 mstore
        0xda0 mload add dup1 0xda0 mstore
        0xdc0 mload add dup1 0xdc0 mstore
        0xde0 mload add dup1 0xde0 mstore
        0xe00 mload add dup1 0xe00 mstore
        0xe20 mload add dup1 0xe20 mstore
        0xe40 mload add dup1 0xe40 mstore
        0xe60 mload add dup1 0xe60 mstore
        0xe80 mload add dup1 0xe80 mstore
        0xea0 mload add dup1 0xea0 mstore
        0xec0 mload add dup1 0xec0 mstore
        0xee0 mload add 0xee0 mstore
        
        // Fourth pass (input): move to buckets
        i := 0x44
    l4:
        temp1 := calldataload(i)
        addr1 := mul(div(temp1, scale), 32)
        addr2 := sub(mload(addr1), 32)
        mstore(addr1, addr2)
        mstore(addr2, temp1)
        i := add(i, 32)
        jumpi(l4, lt(i, calldatasize))
        
        // Fifth pass (buckets): sort buckets
        addr2 := mload(0)
        i := 0x20
    l5:
        addr1 := mload(i)
        jumpi(l5n, lt(addr2, sub(addr1, 32)))
        addr2 := addr1
        i := add(i, 32)
        jumpi(l5, lt(i, 3840))
        jump(l5e)
    l5n:
        sort(addr2, sub(addr1, 32))
        addr2 := addr1
        i := add(i, 32)
        jumpi(l5, lt(i, 3840))
    l5e:
        addr1 := add(sub(calldatasize, 0x44), sub(3840, 32))
        jumpi(l5s, lt(addr2, addr1))
        jump(done)
        
    l5s:
        sort(addr2, addr1)
        
        function sort(lo, hi) {
            
            let lolo := lo
            let hihi := hi
            let d := sub(hi, lo)
            
            if lt(d, 96) {
                
                // Optimize for two
                jumpi(three, gt(d, 32))
                {
                    let a := mload(lo)
                    let b := mload(hi)
                    jumpi(end, gt(b, a))
                    mstore(lo, b)
                    mstore(hi, a)
                end:
                }
                jump(ret)
                
                // Optimize for three
            three:
                {
                    let a := mload(lo)
                    let b := mload(add(lo, 32))
                    let c := mload(hi)
                    jumpi(case1, gt(b, a))
                    a
                    b
                    =: a
                    =: b
                case1:
                    jumpi(case3, gt(c, b))
                    b
                    c
                    =: b
                    =: c
                    jumpi(case3, gt(b, a))
                    a
                    b
                    =: a
                    =: b
                case3:
                    mstore(lo, a)
                    mstore(add(lo, 32), b)
                    mstore(hi, c)
                }
                jump(ret)
            }
            
            // Partition
            {
                let pivot
                let lov
                let hiv
                
                // Compute pivot value
                pivot := and(div(add(lo, hi), 2),
0x0fffffffffffffffffffffffffffffffffffffffffffffffffffffffffffffe0
                )
                pivot := mload(pivot)
                
            loop:
                lov := mload(lo)
                hiv := mload(hi)
                jumpi(loend, iszero(lt(lov, pivot)))
            loloop:
                lo := add(lo, 32)
                lov := mload(lo)
                jumpi(loloop, lt(lov, pivot))
            loend:
                jumpi(hiend, iszero(gt(hiv, pivot)))
            hiloop:
                hi := sub(hi, 32)
                hiv := mload(hi)
                jumpi(hiloop, gt(hiv, pivot))
            hiend:
                jumpi(end, iszero(lt(lo, hi)))
                mstore(lo, hiv)
                mstore(hi, lov)
                lo := add(lo, 32)
                hi := sub(hi, 32)
                jump(loop)
                
            end:
                lo := hi
                hi := add(lo, 32)
            }
            
            // Recurse
            jumpi(sorthi, eq(lolo, lo))
            sort(lolo, lo)
        sorthi:
            jumpi(ret, eq(hi, hihi))
            sort(hi, hihi)
        ret:
        }
        
    done:
        mstore(sub(3840, 0x40), 0x20)
        mstore(sub(3840, 0x20), calldataload(0x24))
        return(sub(3840, 0x40), sub(calldatasize, 4))
        
    trivial:
        calldatacopy(0, 4, calldatasize)
        return(0, sub(calldatasize, 4))
        
    reverse:
        calldatacopy(0, 4, 0x40)
        i := 0x44
        temp1 := sub(calldatasize, 36)
        
    lr:
        mstore(temp1, calldataload(i))
        i := add(i, 32)
        temp1 := sub(temp1, 32)
        jumpi(lr, lt(i, calldatasize))
        
        return(0, sub(calldatasize, 4))
        
    explode:
        selfdestruct(0)
        
    }}
}

```
