# The arrest [13 solves] [494 points]

```
In the dim-lit confines of his room, a lone figure hunched over a computer screen. Known online as 'Swissy', he was one of the most notorious ransomware operators worldwide.
From his small apartment in a forgotten corner of Moscow, Swissy had wreaked havoc on the digital world, crippling entire industries with his ransomware attacks.
But tonight, his reign of terror ended abruptly. A sudden knock echoed through the room, followed by the splintering of the door as Russian FSB agents stormed in.
Swissy was arrested, but the real challenge was only beginning - tracing the syndicate behind him.
Find the address who funded the ransomware operator (0xf6c0513FA09189Bf08e1329E44A86dC85a37c176)
```

Our goal is to find the address that funded 0xf6c0513FA09189Bf08e1329E44A86dC85a37c176

So, just write a script that read all transactions and find a transaction that the `to` field is 0xf6c0513FA09189Bf08e1329E44A86dC85a37c176

```python
from web3 import Web3, HTTPProvider
from web3.middleware import geth_poa_middleware

web3 = Web3(HTTPProvider('http://62.171.185.249:8502/'))
web3.middleware_onion.inject(geth_poa_middleware, layer=0) 

addresses = []

for block in range(1000):
	print(block)
	txns = web3.eth.get_block(block).transactions
	for txn in txns:
		txn = txn.hex()
		txn_data = web3.eth.get_transaction(txn)
		if txn_data['to'] == "0xf6c0513FA09189Bf08e1329E44A86dC85a37c176":
			print(txn_data)
			addresses.append(txn_data['from'])
print("Addresses that funded 0xf6c0513FA09189Bf08e1329E44A86dC85a37c176 :", addresses)
```

```
AttributeDict({'blockHash': HexBytes('0x18b1595ff23477074b4acf370bd8552db6e5fa638af144a833f345b3303a9c7d'), 'blockNumber': 928, 'from': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x49c97be4a735e88a34ea6b0f4f7ca2e86dee67d1b472214b7cf4c703ffdd3c9e'), 'input': '0x', 'nonce': 0, 'to': '0xf6c0513FA09189Bf08e1329E44A86dC85a37c176', 'transactionIndex': 1, 'value': 7892772282257845895, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x05e0ff5174bd9a20b950e55129d42ad006fdeb10362f9fe1e9409d293215abfc'), 's': HexBytes('0x0a00ed76a186f829edc75aa149bb37a390083d1f630b1ae9f7e524574e06d14a')})
```

```
Addresses that funded 0xf6c0513FA09189Bf08e1329E44A86dC85a37c176 : ['0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2']
```

So the address is 0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2, which will be the flag


# Tracing the First Transaction [9 solves] [498 points]

```
The first transaction was the most straightforward, traced back to a bitcoin wallet belonging to a shell company.
The company was a front, but it was a start. They discovered the shell company was registered in the Cayman Islands -
a notorious tax haven, making it a perfect place to launder illicit fund
Follow the trail. Find the address who funded the the shell company (last step's flag)


NOTE : Relevant contracts and addresses can be found in the help section on the blockchain website at the help page
```

Our goal is to find the address that funded the address that is the previous flag `0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2`

So, just write a script that find transaction with the `to` field set to `0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2`

```python
from web3 import Web3, HTTPProvider
from web3.middleware import geth_poa_middleware

web3 = Web3(HTTPProvider('http://62.171.185.249:8502/'))
web3.middleware_onion.inject(geth_poa_middleware, layer=0) 

addresses = []

for block in range(1000):
	print(block)
	txns = web3.eth.get_block(block).transactions
	for txn in txns:
		txn = txn.hex()
		txn_data = web3.eth.get_transaction(txn)
		if txn_data['to'] == "0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2":
			print(txn_data)
			addresses.append(txn_data['from'])
print("Addresses that funded 0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2 :", addresses)
```

```
AttributeDict({'blockHash': HexBytes('0x340e7612a4b3e2dded0f50d4e12ecff48ebbd66751f184dd50f4153c368517d2'), 'blockNumber': 899, 'from': '0xA4dEcDE5c65D94B2A984346dAa5FA37A77F04c2a', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xef082750deea98dff5d4a428e132db67fae1078493010fa4b636364532beb2aa'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 7, 'value': 1416651435277049263, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x39a7c6d366d063a57edb29f4e707fc41f2ea96693a0691cd1c89a2ce700eb74c'), 's': HexBytes('0x1f3c063ed63ba1a8f48e3946f56df58a917c52ee07b4848aff3859f79707a94d')})
900
901
AttributeDict({'blockHash': HexBytes('0x3e86403fbf772e9c4e125a5d87a708654fbd0d882496ac3d6cc6c3fab639bba2'), 'blockNumber': 901, 'from': '0x7D61eB01524570DfE5763393F8c1231058130c92', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xb807950eec66243c930795261a95f838af777302db9fc10e5995d254f38e246f'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 2, 'value': 1011893882340749474, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0xd6a1b71dac665971c0bbb92b77446dbf551587798d9826efead4342af1868501'), 's': HexBytes('0x16b78844c540ac4765e6b986fe6b13ac3d0f4a429d056e603c7aa881bd1f45ac')})
902
AttributeDict({'blockHash': HexBytes('0x50cf5b7aa92708b44575910c3091845d0886901ec1417ba12800a928743ca800'), 'blockNumber': 902, 'from': '0x71693bD81C023374c43D4564a1435f1a1b431C5d', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xa4a83025ea3a208f41d1d5c8bc5487c89a37d303a5a4be6fe2f3c1ae759588d6'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 3, 'value': 1079353474496799439, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x6835088d6bba4a5c83dc3c42b1b6fc99fd9e26e5adf74b1bdcb6c0221c80bf84'), 's': HexBytes('0x60a48b367138cbe872fadd2c0f56b5f1b1feaadb13b07acfc6e0612eae2c13a9')})
903
AttributeDict({'blockHash': HexBytes('0x116189f4c6bfb6ce7e21c805372e2a544b2c1e65d3d8643dfce6e1235e0a8d7b'), 'blockNumber': 903, 'from': '0xF725679450D89f8b00e5Bb998B6aDBF3c2e0f1AD', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xe208825f0f0f9d16de6f0ef49ad05618132da39603651b42cd2af437c8bd7b2d'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 6, 'value': 1034380413059432795, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0xbcf2003a6e3be4888b24598a1866ae7d4086160b45b771f1c36e1296166e9f52'), 's': HexBytes('0x4532021252042554c3337aef0fe7f40aa0f57fb342ce6066680f1311514d4993')})
904
905
AttributeDict({'blockHash': HexBytes('0x0db226cdc389052d7a30f4e3f46b3a0fd40e1f98e18148c98a64d562a1d9268c'), 'blockNumber': 905, 'from': '0x3fe60a1Ed7165b9f1319Ddc5EA12d1A01353a717', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x69af1d532e682e91d38a583850d5e11838cce1c764d6344068a7d5f631825be3'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 3, 'value': 1371678373839682620, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x611ecdde52e577de63c94ca2ae477624b1bcbf79a3b3966e13fbd80dcfd56151'), 's': HexBytes('0x24b9ce7d3f22fbbdbaa818e20ca579d765b535b6660e56e9ee2a29e755f67b94')})
906
AttributeDict({'blockHash': HexBytes('0x6a565b8c4adb64ef2159dd6736d16e967e1617c84b7cfada637d0971dbc1f772'), 'blockNumber': 906, 'from': '0x4f750BD555755Ab66e87B24A2DD28310e37c465c', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x5464fba0cc1501f56a940a3b941ce4806c1137cf34dc1d2046743363a8dcab2d'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 12, 'value': 719568982997866292, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x20551d4273d7584df55ad0631bee68def95fcf536c3772eaef052e87f3c5d78a'), 's': HexBytes('0x327a77cf396c00c9155438170ae6462ad93a49df8fd2aa18bd5b5335d5774b9b')})
907
AttributeDict({'blockHash': HexBytes('0x6bf3e78c870385edb5ccbef35b0c530a60b251b24af10bdf224ab5d88e000a0e'), 'blockNumber': 907, 'from': '0x25540369aE41F5B1a71ddA1dEff53311CDaB3b1B', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x9bb047b6a26fda35e02f2e768b613937e7f9bc9aaeee9af095ffa679ca464bb3'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 3, 'value': 1169299597371532725, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x9673c82b78427a9d4ed28fcaff21aef80554d0e7c3bf1772509623148595bece'), 's': HexBytes('0x64ab708970c115729c78c779f339cbdbf714e452611fe3445b5493e48e73451d')})
908
909
AttributeDict({'blockHash': HexBytes('0x4872a64877dea6df1a4e325a67b7224b11c88fc12174b93a4e4854b129a4c9a0'), 'blockNumber': 909, 'from': '0x8c98DDf14543cC791bA2434b4981B429bF3132c4', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x7140bfa5b05123a2b34b6c7e0551a0c4a18536e5463eac125e9769f1faddb583'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 2, 'value': 944434290184699509, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x95923758c4c2bc65462009971690dbaad767f8dcfa6a761becb8e8679b3b47c9'), 's': HexBytes('0x0865bfe7ad675743e01191127a22ba85cb6dbb82439537d406735ca782b07f6d')})
910
AttributeDict({'blockHash': HexBytes('0x8631787e9ecd00f4829ad2c9fd33dbf31bd5e0aaa72f003fedeb6b7e41a28f33'), 'blockNumber': 910, 'from': '0xBEe1C37253095EDFb677D9DFae301293D9d4F18a', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x94f0ced261e7309518754595fa4e253f1aff2ef286ddabfb5badbb6fb9c32e32'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 20, 'value': 876974698028649544, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x78ef122ae18a36811bbddda40952cf71d043cc87a31c65a2d024cd6e826e26df'), 's': HexBytes('0x598be0ecbc319c1e0d03554b20991a3f883fc91d30119d205f788693a637cc64')})
911
AttributeDict({'blockHash': HexBytes('0xb2da6617497f1fca7062249a4b70dcfbcb7c680bc4c31f3952ae9887e63a4055'), 'blockNumber': 911, 'from': '0xf8C7c772F13c75BE6B9f854Bd12C99c8cE19D2d2', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x4ffe1a49d3bc105e72391a5dfc50911c11eaa4dd5a41ded736e20c8f1a0bff23'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 1, 'value': 1664003273182565802, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x0dd8025a2b8060e7e149633300dea0dfe7888434d73230b559a234fc0c3de1ab'), 's': HexBytes('0x015c4291164dcfdf7516f78ecd19d8618b278b44e497340cab60a966fe64bc2f')})
912
AttributeDict({'blockHash': HexBytes('0xc8aa11ee69a23cd97a87876948e0bb85d5394bd21efbf8bad944e145b06be5ef'), 'blockNumber': 912, 'from': '0x290B7365F8D771d76Cc8ff8aae1C7DF858bf9AEc', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x6f9f8f1a31fd819d541e7d85b2e9119eb0d7167d955974901f33d6018da63880'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 5, 'value': 1371678373839682620, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0xf8799f07108abed0bf5601be8527fe85475482b5ff3362a4df7712e63df0da9c'), 's': HexBytes('0x7f864457597aed7eea6f97e2a0b3df200b0dce7881e2815b2cb981b52be55159')})
913
AttributeDict({'blockHash': HexBytes('0x00d2a5b61f79a5aac5c89d741a436e3447814e52ced9f907bc57ac63176fe02b'), 'blockNumber': 913, 'from': '0x46bACe73B5e743a83ea20dE05C4CE8191aae8c05', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xa5525beb99e7a23fb0ecaedb78dd4415f8670ee1370fd30f5a1ad9758448144f'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 3, 'value': 1753949396057299088, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x4ff2e61787d5b3296e40b18c40795981c36d9017cf5b79e59db117f5ef9d7af2'), 's': HexBytes('0x1436fd996cb78a3a1f69e68ccff1d671015888a53fae227817e6b5a3796d6dc5')})
914
AttributeDict({'blockHash': HexBytes('0xd755fdb343ea21917acaae92aba54272831b63171550f32db2888840dbc02c43'), 'blockNumber': 914, 'from': '0xbDF024D8504C630106d18acD6d470C96B326a7aB', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x89a54d045fd32437d9e50aacfbda2bbc540cfc28bb76093eea6ecb1001d11678'), 'input': '0x', 'nonce': 0, 'to': '0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2', 'transactionIndex': 22, 'value': 1371678373839682620, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x4811a17f7f56a80ec42e5d8847387b5206fdaf5aa80f357747cdb718a454f342'), 's': HexBytes('0x52140cb68997279da20ed475cc6406a7b764920aa0731f34c526337640b83086')})
```

```
Addresses that funded 0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2 : ['0xA4dEcDE5c65D94B2A984346dAa5FA37A77F04c2a', '0x7D61eB01524570DfE5763393F8c1231058130c92', '0x71693bD81C023374c43D4564a1435f1a1b431C5d', '0xF725679450D89f8b00e5Bb998B6aDBF3c2e0f1AD', '0x3fe60a1Ed7165b9f1319Ddc5EA12d1A01353a717', '0x4f750BD555755Ab66e87B24A2DD28310e37c465c', '0x25540369aE41F5B1a71ddA1dEff53311CDaB3b1B', '0x8c98DDf14543cC791bA2434b4981B429bF3132c4', '0xBEe1C37253095EDFb677D9DFae301293D9d4F18a', '0xf8C7c772F13c75BE6B9f854Bd12C99c8cE19D2d2', '0x290B7365F8D771d76Cc8ff8aae1C7DF858bf9AEc', '0x46bACe73B5e743a83ea20dE05C4CE8191aae8c05', '0xbDF024D8504C630106d18acD6d470C96B326a7aB']
```

There are many addresses that funded 0x54741632BE9F6E805b64c3B31f3e052e1eAe73e2, so we will write another script to trace addresses that funded those addresses

```python
from web3 import Web3, HTTPProvider
from web3.middleware import geth_poa_middleware

web3 = Web3(HTTPProvider('http://62.171.185.249:8502/'))
web3.middleware_onion.inject(geth_poa_middleware, layer=0) 

addresses = ['0xA4dEcDE5c65D94B2A984346dAa5FA37A77F04c2a', '0x7D61eB01524570DfE5763393F8c1231058130c92', '0x71693bD81C023374c43D4564a1435f1a1b431C5d', '0xF725679450D89f8b00e5Bb998B6aDBF3c2e0f1AD', '0x3fe60a1Ed7165b9f1319Ddc5EA12d1A01353a717', '0x4f750BD555755Ab66e87B24A2DD28310e37c465c', '0x25540369aE41F5B1a71ddA1dEff53311CDaB3b1B', '0x8c98DDf14543cC791bA2434b4981B429bF3132c4', '0xBEe1C37253095EDFb677D9DFae301293D9d4F18a', '0xf8C7c772F13c75BE6B9f854Bd12C99c8cE19D2d2', '0x290B7365F8D771d76Cc8ff8aae1C7DF858bf9AEc', '0x46bACe73B5e743a83ea20dE05C4CE8191aae8c05', '0xbDF024D8504C630106d18acD6d470C96B326a7aB']

funded_by = []

for block in range(860,1000):
	print(block)
	txns = web3.eth.get_block(block).transactions
	for txn in txns:
		txn = txn.hex()
		txn_data = web3.eth.get_transaction(txn)
		if txn_data['to'] in addresses:
			print(txn_data)
			funded_by.append(txn_data['from'])
print("Addresses that funded those addresses :", funded_by)
```

```
AttributeDict({'blockHash': HexBytes('0xada6e18c3c193ffac85e0691ceb19bf08248cfe1ce0bb26e21052e85b8abb17f'), 'blockNumber': 879, 'from': '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x32489306c9d53e2c857b3c0277bf5cf031d7a6ae81fa0392bbfd8c666084caf1'), 'input': '0x', 'nonce': 0, 'to': '0xA4dEcDE5c65D94B2A984346dAa5FA37A77F04c2a', 'transactionIndex': 7, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0xed2a9850c4162368a18f8711892ff4947d92f1d63abf72ed9dd16bdbb4890665'), 's': HexBytes('0x5a8879b2bd8eb24272516ba04a01810d898b0d82e6de5328e0c7ef9f038e9442')})
880
AttributeDict({'blockHash': HexBytes('0x85aeceb678758c01b1d4843083bb1e1fe38910b0fd09e548bbbaf339e5caaa79'), 'blockNumber': 880, 'from': '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xad91414b686d9ea284d56cc9ae168a0538b7333193082a5fa7ca82a7f58ae2aa'), 'input': '0x', 'nonce': 1, 'to': '0x7D61eB01524570DfE5763393F8c1231058130c92', 'transactionIndex': 4, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x0109b93bee8f1d808639563d5b2374357f79df6b4da945eb7c6764d9aa838db0'), 's': HexBytes('0x0c3226764730bf05d98b9eed92b88d7624baf0192ffbce7dd638265840825fbf')})
881
AttributeDict({'blockHash': HexBytes('0x8962e3211860ddd88ae97cee8d9a84178da032911455d595753a3a20c6bd15b9'), 'blockNumber': 881, 'from': '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xa6af37c109b742000dd3e5d13b6bbd828fdb92fae1491cde969414d51ced4187'), 'input': '0x', 'nonce': 2, 'to': '0x71693bD81C023374c43D4564a1435f1a1b431C5d', 'transactionIndex': 6, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0xc33bf20124aca10355e2928987b495736270ac276540c4dc25ee18b7187368a3'), 's': HexBytes('0x37acd9a860aeafd38a1d62b4de2513c2edff4aa03275cb6c993f4104c1ef02c1')})
882
883
AttributeDict({'blockHash': HexBytes('0x3c30c2801cff41dd36c632a9b36eaeff4ae2e910b756b32bc185a0c1cd02f6e4'), 'blockNumber': 883, 'from': '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x2d9791cf06820ed5fcabc88b22e0a7e9e34901c3c4e9d328afcc071e72468e06'), 'input': '0x', 'nonce': 3, 'to': '0xF725679450D89f8b00e5Bb998B6aDBF3c2e0f1AD', 'transactionIndex': 4, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0xd21b9d0a8bfef270b64ef4758efac90afa483b78ec9fe0c52886b3fd088d3156'), 's': HexBytes('0x09d0355469d3737d58678a5e8ca45c83923465a4149b77546533a1ef268b70d0')})
884
AttributeDict({'blockHash': HexBytes('0x8e43be0962174093d5aa558cca7b1c4fc1a11bab63b9e15bef4666ed8f998ba7'), 'blockNumber': 884, 'from': '0xFfb0c469144D9F187E77C6089B1a882A1E6e832C', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x8c7951d919bb4cb2716e701906bf19e4ca30fc32c0060ea0ae475198e9db5a34'), 'input': '0x', 'nonce': 0, 'to': '0x3fe60a1Ed7165b9f1319Ddc5EA12d1A01353a717', 'transactionIndex': 6, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x88c136c6bf9ffb21c9639b3504a1f77d4468fc2453aeef20c572e33e5fe5e8cf'), 's': HexBytes('0x3192c7f017588fd4aaa3e9aba1a0cc845c29d2336720cfd6ad93aadff22e7946')})
885
886
887
AttributeDict({'blockHash': HexBytes('0x804155ddb9c1819799b36cd7135c20364c6216ac73d149aa33e89ff5271edd3a'), 'blockNumber': 887, 'from': '0xFfb0c469144D9F187E77C6089B1a882A1E6e832C', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x02ecf4d3d793b72a593f2c9ee0c76546e6bbdc83d6d277f23830936901850e27'), 'input': '0x', 'nonce': 2, 'to': '0x4f750BD555755Ab66e87B24A2DD28310e37c465c', 'transactionIndex': 4, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x435e7a1ec4a0e018832e46af258b611c1abe832a736ab6ab3825b6c9b7539ed3'), 's': HexBytes('0x0a11d2536ed27e99cc42afd83a48ba12906759bb93112e20ebe0978d0ebc5b51')})
888
889
AttributeDict({'blockHash': HexBytes('0xf86005b7f50d388d5399f64c253214bfa084bc827d40c1fd51f7a9d80ba4a6c7'), 'blockNumber': 889, 'from': '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xf8a83db512e5fa8023c2364f6dc64be5b11fee0f934b93e3b2eac2d5ff7100f8'), 'input': '0x', 'nonce': 0, 'to': '0x25540369aE41F5B1a71ddA1dEff53311CDaB3b1B', 'transactionIndex': 1, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x738fe71d2b099f4ff0738582ab04a0444cab31e6d015e93e63519412385d9b14'), 's': HexBytes('0x36345b53deb678e124e43f4abc77ebc6dcc5162deff22ddd78916e181bcc51b2')})
890
891
892
AttributeDict({'blockHash': HexBytes('0x6477a766c704a4804f13aebea6da2f7af017d2f414d3c5bf362507abcc737d46'), 'blockNumber': 892, 'from': '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x451c27d0daf3ab27281133ff6b8e9c30360b04a40a53355d3294a71a4374ce23'), 'input': '0x', 'nonce': 2, 'to': '0x8c98DDf14543cC791bA2434b4981B429bF3132c4', 'transactionIndex': 4, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x2634b45d3a7004ad461bfd2e623e9b0a6463149fd04bef362fa44a96fafb6b6b'), 's': HexBytes('0x32dd8bf82b722454ce8bb6381f977a673cdb1f76cb3a4ff15ab13963d7b34ad6')})
893
AttributeDict({'blockHash': HexBytes('0x27ba9e022f137618b28d29fe5e82595fdf759ff12be705f20871bf6aa43495a3'), 'blockNumber': 893, 'from': '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xeeca3ec2c8706dd3d359dd06851b2468e022fe47bf1c2e9a0a6433e39a11427d'), 'input': '0x', 'nonce': 3, 'to': '0xBEe1C37253095EDFb677D9DFae301293D9d4F18a', 'transactionIndex': 7, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x5a6e0e85b1f41c6c13d56b38d7cf5d86243129b148b2f3b96e7cb3aab7df5179'), 's': HexBytes('0x307784c8dce8588f69caf9af6356d91eae925ddca8e83ed4f4fa830a1200355e')})
894
AttributeDict({'blockHash': HexBytes('0xe797e5ebdf25b0a36ac41f971d2258067da6ba4bd1b3959fee39709ca5b13c3f'), 'blockNumber': 894, 'from': '0xe9fD7987d94596E70450e184a30176FEef440EbE', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x53d5a58f7905084c4426e3c0010757353ec1a9222b226d164f1993cdc766c760'), 'input': '0x', 'nonce': 0, 'to': '0xf8C7c772F13c75BE6B9f854Bd12C99c8cE19D2d2', 'transactionIndex': 6, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x30f1e056ce946356f7edf570fd1089b0e4e5f098a472f3c2ea9f33982d372dc7'), 's': HexBytes('0x6cce5cfd88cef0cd69e12bb9cb1c17b78a8de5c2f05828a5a010d365c9d4cde0')})
895
896
AttributeDict({'blockHash': HexBytes('0xbc911d0154762bb52cc2536ef2828727d5b85eb9a249996b4f66221c2236c0e5'), 'blockNumber': 896, 'from': '0xe9fD7987d94596E70450e184a30176FEef440EbE', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x3de01c3b928c47913f84b5b39d0866c9839780108ad58f48524ae93177a827e7'), 'input': '0x', 'nonce': 1, 'to': '0x290B7365F8D771d76Cc8ff8aae1C7DF858bf9AEc', 'transactionIndex': 8, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x3ce0c6e197d063de4fbaeac71d0e4d59bc0a45caec3204b9dba32f0839721260'), 's': HexBytes('0x24a09bc1eb258d518a7cba7e7b9289472a8226c3fd4bb0b68d88cffe57a982ae')})
897
AttributeDict({'blockHash': HexBytes('0x4f7f3a848bb677f4e40d8e333a8846deab0dee7a9415175ede75eaad1dcd3a4b'), 'blockNumber': 897, 'from': '0xe9fD7987d94596E70450e184a30176FEef440EbE', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xbd065f0db22f6d81ce91e13791dcc0a2470558d103f03608d1935a4ad419657e'), 'input': '0x', 'nonce': 2, 'to': '0x46bACe73B5e743a83ea20dE05C4CE8191aae8c05', 'transactionIndex': 2, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x9090ae19ae69b115846f9b0bf6dc4a3bd2c257ce834ffb6a5bf918aa8ac0d346'), 's': HexBytes('0x633cd305554fc8c3a0239096fe8b5f45880a711fe5b0c8015101a41695481c1a')})
898
AttributeDict({'blockHash': HexBytes('0x2dc8513bc80a915ffabe47f0ff23d759b3bc4df90de89c36776b149e8e5dd9d5'), 'blockNumber': 898, 'from': '0xe9fD7987d94596E70450e184a30176FEef440EbE', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xbe6f952b6c44d4d71a3a6e912e916249dd6e057c631021785ff059ee8bc76e76'), 'input': '0x', 'nonce': 3, 'to': '0xbDF024D8504C630106d18acD6d470C96B326a7aB', 'transactionIndex': 6, 'value': 2248653071868332165, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x5685f0ed415e5a177cab98dfc89e4a0a470243ee4ebda9596c20f55759f48c27'), 's': HexBytes('0x791ce423a22e6db0a1d13e1934c69c51b877aa9cf331ca499f25a3714c320e02')})
```

```
Addresses that funded those addresses : ['0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0xFfb0c469144D9F187E77C6089B1a882A1E6e832C', '0xFfb0c469144D9F187E77C6089B1a882A1E6e832C', '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', '0xe9fD7987d94596E70450e184a30176FEef440EbE', '0xe9fD7987d94596E70450e184a30176FEef440EbE', '0xe9fD7987d94596E70450e184a30176FEef440EbE', '0xe9fD7987d94596E70450e184a30176FEef440EbE']
```

This time there are some duplicate addresses, so just write another script to trace the address that funded those addresses

```python
from web3 import Web3, HTTPProvider
from web3.middleware import geth_poa_middleware

web3 = Web3(HTTPProvider('http://62.171.185.249:8502/'))
web3.middleware_onion.inject(geth_poa_middleware, layer=0) 

addresses = ['0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', '0xFfb0c469144D9F187E77C6089B1a882A1E6e832C', '0xFfb0c469144D9F187E77C6089B1a882A1E6e832C', '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', '0xe9fD7987d94596E70450e184a30176FEef440EbE', '0xe9fD7987d94596E70450e184a30176FEef440EbE', '0xe9fD7987d94596E70450e184a30176FEef440EbE', '0xe9fD7987d94596E70450e184a30176FEef440EbE']


funded_by = []

for block in range(860,1000):
	print(block)
	txns = web3.eth.get_block(block).transactions
	for txn in txns:
		txn = txn.hex()
		txn_data = web3.eth.get_transaction(txn)
		if txn_data['to'] in addresses:
			print(txn_data)
			funded_by.append(txn_data['from'])
print("Addresses that funded those addresses :", funded_by)
```

```
AttributeDict({'blockHash': HexBytes('0x8b63cc0dbed11bc369b537b7b24b05b51aacf88813e45d03b9ea9d4872ebf6da'), 'blockNumber': 875, 'from': '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xddf8df8a84b703184c06e1b0c5b21b0c91d4ed45a52e8406abd36dd138e51300'), 'input': '0x', 'nonce': 47, 'to': '0x2626e2C853b1F94c012DF03b595BDfb212914b0f', 'transactionIndex': 16, 'value': 44973061437366643312, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0xabd4c343cb76693768c76b2b05b6fe4974754f12555d60b5032fc61620299e89'), 's': HexBytes('0x174ce5305a4112a5eddd0e07521891232ebab13e5d4599663e128b7840847eff')})
876
AttributeDict({'blockHash': HexBytes('0xebdf8a680b5eaa280ad2a7bcee823cfccab8aeb3e75d7d36616c9c6c67918710'), 'blockNumber': 876, 'from': '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0xa294ce12f316a48d21781454d9eecfddeef5ee241e25194e741b7c1db159846b'), 'input': '0x', 'nonce': 48, 'to': '0xFfb0c469144D9F187E77C6089B1a882A1E6e832C', 'transactionIndex': 3, 'value': 44973061437366643312, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0x39f29d1249d7e55426d6e574bf9a93462e9b48374b9b695e021cc2d06d2c2db7'), 's': HexBytes('0x4a08a7427222b7fd3da55d27681722d6ab01669bd8043d3303f01a5997fc78fe')})
877
AttributeDict({'blockHash': HexBytes('0xeb5c22578eb27890c9399038da9ee9e582245f337526fdf51821bc8039086ef9'), 'blockNumber': 877, 'from': '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x46c6076cee627d8ca854c2bb0ebd64201988a1b41612072b816c77379c8f98c3'), 'input': '0x', 'nonce': 49, 'to': '0xe08384432816c029CC8365Df4B2d09fdFaC8640f', 'transactionIndex': 8, 'value': 44973061437366643312, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x80f350c619e58477bca4b0d0a9a2400acb3b482f2af35e2bf88e61f030df588a'), 's': HexBytes('0x7f80cddb9b0bf21ec5740efc2293877c474ee4e82ed6718504b66f87def7c4df')})
878
AttributeDict({'blockHash': HexBytes('0x4de5bc551cac17f4beffe79686aaca56531a7685a0f4301e043e72ea29bf9d23'), 'blockNumber': 878, 'from': '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', 'gas': 21000, 'gasPrice': 42, 'hash': HexBytes('0x3447e6bf496826662f37770c3063028381f0621ff2cc6a815a4fddb7a64ae40d'), 'input': '0x', 'nonce': 50, 'to': '0xe9fD7987d94596E70450e184a30176FEef440EbE', 'transactionIndex': 6, 'value': 44973061437366643312, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0xcff9bfff4fbe85c5bbeacf1c1718d3611b88132229ff475dd7afb31d1b95a286'), 's': HexBytes('0x4212ec65bd4fb1c9851860dfb78922b43631b876ef2f222491cd67d2e66f52e9')})
```

```
Addresses that funded those addresses : ['0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353']
```

Finally, it is only one address, so this is the flag `0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353`


# The Second Transaction and the Offshore Connection [7 solves] [499 points]

```
The second transaction led to an offshore exchange account in Switzerland, further complicating matters.
After weeks of negotiations, they managed to obtain the identity of the account holder, a businessman based in Shanghai. They were one step closer, but still far from the truth. Follow the trail. Find the address who funded the the exchange's account (last step's flag)
```

So we have to find the address that funded the address that is previous challenge's flag : `0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353`

```python
from web3 import Web3, HTTPProvider
from web3.middleware import geth_poa_middleware

web3 = Web3(HTTPProvider('http://62.171.185.249:8502/'))
web3.middleware_onion.inject(geth_poa_middleware, layer=0) 

addresses = []

for block in range(500, 1000):
	print(block)
	txns = web3.eth.get_block(block).transactions
	#print(txns)
	for txn in txns:
		txn = txn.hex()
		txn_data = web3.eth.get_transaction(txn)
		#print(txn_data['to'])
		if txn_data['to'] == "0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353":
			print(txn_data)
			addresses.append(txn_data['from'])
print("Addresses that funded 0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353 :", addresses)
```

```
AttributeDict({'blockHash': HexBytes('0xe8d9e0f8dd96b932e73712b288485850150202181592afee12db2be3123f7c54'), 'blockNumber': 605, 'from': '0xE255CAaD9B3e531AB5e5772856642Ea8E54Dcc2E', 'gas': 21000, 'gasPrice': 1000000000, 'hash': HexBytes('0xe247627e0209e8018f407f95d1e74490bb314d7e34cb07617a87d8dbb3809593'), 'input': '0x', 'nonce': 8, 'to': '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', 'transactionIndex': 1, 'value': 8485132000000000000, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x73fa30773cd719588408b2f030a807a0a2f0e35b324c55208cc249b090ed4e6b'), 's': HexBytes('0x3beb7c7271462e2d29fa86b015cb653e995b8472ebb3abb2a4fd67c8ba4232d6')})
```

```
Addresses that funded 0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353 : ['0xE255CAaD9B3e531AB5e5772856642Ea8E54Dcc2E']
```

There is only one transaction, however it is not the flag

The help page mentioned that address, which it is the CEX address
```
OSINT / Blockchain :

All of the OSINT transactions took place before the CTF begun. Please filter accordingly so as to not be cluttered by parasite transactions that may emerge.
CEX Address : 0xE255CAaD9B3e531AB5e5772856642Ea8E54Dcc2E.
All transactions from this address are considered CEX transactions (exactly as a user would withdraw coins from Bybit for example)
```

So, maybe it will be a token transfer instead, so I will write a script that search the calldata which contains that address

```python
from web3 import Web3, HTTPProvider
from web3.middleware import geth_poa_middleware

web3 = Web3(HTTPProvider('http://62.171.185.249:8502/'))
web3.middleware_onion.inject(geth_poa_middleware, layer=0) 

transactions = []

for block in range(1000):
	print(block)
	txns = web3.eth.get_block(block).transactions
	for txn in txns:
		txn = txn.hex()
		txn_data = web3.eth.get_transaction(txn)
		if "26F8A2D63B06D84121b35990ce8b7FEbac4Fe353".lower() in txn_data['input'].lower():
			print(txn_data)
			transactions.append(txn_data['hash'].hex())
print("Calldata contains 0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353 :", transactions)
```

```
610
AttributeDict({'blockHash': HexBytes('0x9b74702d78ebd265a1fa75851d6cf94f4c78584672709a02460e450ad0e87e69'), 'blockNumber': 610, 'from': '0x80AF38eCD0dE67B02552A558cFD144a38D544160', 'gas': 7023320, 'gasPrice': 1000000000, 'hash': HexBytes('0x156b175ed702705b1dd2ee157ee003ef08dc1d23407b95de7cd259721e6c8edf'), 'input': '0xa9059cbb00000000000000000000000026f8a2d63b06d84121b35990ce8b7febac4fe35300000000000000000000000000000000000000000000d1a401ee0332eec00000', 'nonce': 30, 'to': '0x036FCE4151658a8fCb6B077A386F7FafAe107975', 'transactionIndex': 1, 'value': 0, 'type': '0x0', 'chainId': '0x539', 'v': 2710, 'r': HexBytes('0xc8ead239c34416cc9dbd010d09853a287b9242e3d2cdbb43694f7c2783d81877'), 's': HexBytes('0x7d39a4df4d3c05fbcb7022d98506d678dfe586b00a9c74b22558e3f77e182740')})
AttributeDict({'blockHash': HexBytes('0x9b74702d78ebd265a1fa75851d6cf94f4c78584672709a02460e450ad0e87e69'), 'blockNumber': 610, 'from': '0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353', 'gas': 7027544, 'gasPrice': 1000000000, 'hash': HexBytes('0xc579f4a2f1608c90681bbaeb8e77e8c7747325cc84f20790ebea6197ca23bd70'), 'input': '0x5c11d79500000000000000000000000000000000000000000000d1a401ee0332eec00000000000000000000000000000000000000000000000000000dbd19305c766489600000000000000000000000000000000000000000000000000000000000000a000000000000000000000000026f8a2d63b06d84121b35990ce8b7febac4fe35300000000000000000000000000000000000000000000000000000000645cd4b20000000000000000000000000000000000000000000000000000000000000002000000000000000000000000036fce4151658a8fcb6b077a386f7fafae107975000000000000000000000000d1a68d5bfa15cb375552492993fc38396eebc1b1', 'nonce': 0, 'to': '0x3038ae6af5726f685B551266d8cCd704e7e0c3CA', 'transactionIndex': 2, 'value': 0, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0xd04399481712934c84c15e2d7e234a04d429db664412d6614ddcf294a73d535e'), 's': HexBytes('0x66054f48c6c1642d99ee872e907086b1f26ec387fd719f3ed437e5d9927da9c1')})
```

There are 2 transactions that the calldata contains that address, in the following blocks that 2 transactions keep repeating, but the calldata is the same

So I just try to decode the calldata with foundry cast

```
# cast 4byte-decode 0xa9059cbb00000000000000000000000026f8a2d63b06d84121b35990ce8b7febac4fe35300000000000000000000000000000000000000000000d1a401ee0332eec00000
1) "transfer(address,uint256)"
0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353
990000000000000000000000 [9.9e23]

# cast 4byte-decode 0x5c11d79500000000000000000000000000000000000000000000d1a401ee0332eec00000000000000000000000000000000000000000000000000000dbd19305c766489600000000000000000000000000000000000000000000000000000000000000a000000000000000000000000026f8a2d63b06d84121b35990ce8b7febac4fe35300000000000000000000000000000000000000000000000000000000645cd4b20000000000000000000000000000000000000000000000000000000000000002000000000000000000000000036fce4151658a8fcb6b077a386f7fafae107975000000000000000000000000d1a68d5bfa15cb375552492993fc38396eebc1b1
1) "swapExactTokensForTokensSupportingFeeOnTransferTokens(uint256,uint256,address[],address,uint256)"
990000000000000000000000 [9.9e23]
15839603017468233878 [1.583e19]
[0x036FCE4151658a8fCb6B077A386F7FafAe107975, 0xd1a68d5bFa15CB375552492993fC38396eebC1b1]
0x26F8A2D63B06D84121b35990ce8b7FEbac4Fe353
1683805362 [1.683e9]
```

The first one is transfer(), and second one is swapExactTokensForTokensSupportingFeeOnTransferTokens()

The second one is sent by that address itself, so we can ignore it, and first one is an ERC20 token transfer of this token : `0x036FCE4151658a8fCb6B077A386F7FafAe107975`, and the sender is `0x80AF38eCD0dE67B02552A558cFD144a38D544160`

So, `0x80AF38eCD0dE67B02552A558cFD144a38D544160` is the flag


# The Third Transaction and the Insider [7 solves] [499 points]

```
The third transaction was the most challenging yet. We have no clue who this belongs to ...
You have to keep following the trail. The end is near !
The address that funded the unknown wallet address should lead to a CEX wallet transfer.
Find the hash of the transaction from the CEX to the main funding address
```

So we can just write a script to search for transaction from/to to CEX address

```python
from web3 import Web3, HTTPProvider
from web3.middleware import geth_poa_middleware

web3 = Web3(HTTPProvider('http://62.171.185.249:8502/'))
web3.middleware_onion.inject(geth_poa_middleware, layer=0) 

transactions = []

for block in range(1000):
	print(block)
	txns = web3.eth.get_block(block).transactions
	#print(txns)
	for txn in txns:
		txn = txn.hex()
		txn_data = web3.eth.get_transaction(txn)
		#print(txn_data['to'])
		if txn_data['to'] == "0xE255CAaD9B3e531AB5e5772856642Ea8E54Dcc2E" or txn_data['from'] == "0xE255CAaD9B3e531AB5e5772856642Ea8E54Dcc2E":
			print(txn_data)
			transactions.append(txn_data)
print("Transactions from/to 0xE255CAaD9B3e531AB5e5772856642Ea8E54Dcc2E :", transactions)
```

This is the first transaction from CEX :
```
471
AttributeDict({'blockHash': HexBytes('0x96f5c57d6ac815b8cafcbbab8bbd9aa63cc45e41edb5540e354614e985e96a64'), 'blockNumber': 471, 'from': '0xE255CAaD9B3e531AB5e5772856642Ea8E54Dcc2E', 'gas': 21000, 'gasPrice': 1000000000, 'hash': HexBytes('0x10110b38d8552bf2d47c958a201da2aa4d184f87cf8a6ef3f5dc57ef9c18162a'), 'input': '0x', 'nonce': 0, 'to': '0x99aC88390Cd2D02Ab8587404f8d90a09425Ed101', 'transactionIndex': 1, 'value': 40000000000000000000000, 'type': '0x0', 'chainId': '0x539', 'v': 2709, 'r': HexBytes('0x33e43961134fd10db8d9f859f6f23a2a0f67d2d7bce50fa70af579ddf98ab6cb'), 's': HexBytes('0x643500078f70203c65b6fee3ec904c333de9331e19d9bb53eeb033aa2aafea7b')})
```

and `0x10110b38d8552bf2d47c958a201da2aa4d184f87cf8a6ef3f5dc57ef9c18162a` is the flag
