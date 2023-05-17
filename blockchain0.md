# Challenge 00 : Oh sh. Here we go again ? [18 solves] [487 points]

```
This year, we take it a step further ! Deploy the contract, understand it, bypass it, verify it !
```

The source code is not given in this challenge, so we can just deploy the challenge contract and get the runtime bytecode with foundry cast :

```
# cast code 0x752d16368bBD81FA5A491522FA18942528A4279E --rpc-url http://62.171.185.249:8502/
0x608060405234801561001057600080fd5b50600436106100575760003560e01c80633c5269d81461005c578063459a279014610066578063473ca96c1461008457806375ec067a146100a2578063e9a37061146100ac575b600080fd5b6100646100ca565b005b61006e6100e6565b60405161007b9190610184565b60405180910390f35b61008c6100f9565b6040516100999190610184565b60405180910390f35b6100aa610127565b005b6100b4610158565b6040516100c19190610184565b60405180910390f35b60016000806101000a81548160ff021916908315150217905550565b600060019054906101000a900460ff1681565b60008060009054906101000a900460ff1680156101225750600060019054906101000a900460ff165b905090565b60008054906101000a900460ff1615610156576001600060016101000a81548160ff0219169083151502179055505b565b60008054906101000a900460ff1681565b60008115159050919050565b61017e81610169565b82525050565b60006020820190506101996000830184610175565b9291505056fea2646970667358221220410b70e32943a11347a432c640d65c5ccae8006bc0dbc6dd7ba624c219c0059264736f6c63430008110033
```

Then we can decompile it with this : 

https://library.dedaub.com/decompile

```
// Decompiled by library.dedaub.com
// 2023.05.12 19:25 UTC
// Compiled using the solidity compiler version 0.8.17


// Data structures and variables inferred from the use of storage instructions
uint256 _win; // STORAGE[0x0] bytes 0 to 0
uint256 stor_0_1_1; // STORAGE[0x0] bytes 1 to 1



function () public payable { 
    revert();
}

function 0x3c5269d8() public payable { 
    _win = 1;
}

function 0x459a2790() public payable { 
    return stor_0_1_1;
}

function win() public payable { 
    v0 = v1 = _win;
    if (v1) {
        v0 = stor_0_1_1;
    }
    return bool(v0);
}

function 0x75ec067a() public payable { 
    if (_win) {
        stor_0_1_1 = 1;
    }
}

function 0xe9a37061() public payable { 
    return _win;
}

// Note: The function selector is not present in the original solidity code.
// However, we display it for the sake of completeness.

function __function_selector__(bytes4 function_selector) public payable { 
    MEM[64] = 128;
    require(!msg.value);
    if (msg.data.length >= 4) {
        if (0x3c5269d8 == function_selector >> 224) {
            0x3c5269d8();
        } else if (0x459a2790 == function_selector >> 224) {
            0x459a2790();
        } else if (0x473ca96c == function_selector >> 224) {
            win();
        } else if (0x75ec067a == function_selector >> 224) {
            0x75ec067a();
        } else if (0xe9a37061 == function_selector >> 224) {
            0xe9a37061();
        }
    }
    ();
}
```

We can set win() to 1 with 0x3c5269d8() and set stor_0_1_1 to 1 with 0x75ec067a() after win is set to 1

```solidity
pragma solidity ^0.8.0;

contract chall0_solve {

    constructor(address chall0) public {
        chall0.call(abi.encodeWithSelector(hex"3c5269d8"));
        chall0.call(abi.encodeWithSelector(hex"75ec067a"));
    }
}
```

Flag :
```
Hero{M3l_weLComes_U_B4cK!:)}
```