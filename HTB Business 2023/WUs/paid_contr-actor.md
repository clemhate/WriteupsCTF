#HTB Business 2023 : Paid_Contr-Actor (Web3)
***

```solidity
// SPDX-License-Identifier: UNLICENSED
pragma solidity 0.8.18;


contract Contract {
    
    bool public signed;

    function signContract(uint256 signature) external {
        if (signature == 1337) {
            signed = true;
        }
    }

}
```
Shouldn't be too hard, we only have to call the function ```signContract(uint256 signature)``` with 1337 as parameter.

```bash
klm@KLM:~$ cast send $TAR "signContract(uint256)" 1337 --rpc-url=$RPC --private-key=$PK
```

Enjoy ! ```HTB{XXXXXXXXXX}```