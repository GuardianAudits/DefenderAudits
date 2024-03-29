![](https://i.imgur.com/zGzXYfT.jpg)


__Audit Firm__: Solidity Lab

__Client Firm__: Poodl

__Prepared By:__ zaskoh, Atarpara, ladboy233

__Delivery Date:__ March 7th, 2023

<br />

__Poodl__ engaged Solidity Lab to review the security of its Smart Contract system. From __February 25th, 2023__ to __March 7th, 2023__, a team of __3__ auditors reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | RaisinLabs                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | https://github.com/poodlTech/tokenAudit                             |
| Commit       | [eebe267b3fdd75a82e09cc270b3c046b2c9f2c84](https://github.com/poodlTech/tokenAudit/tree/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84) |


| Delivery Date     | March 7th, 2023       |
|-------------------|--------------------------------|
| Audit Methodology | Static Analysis, Manual Review |


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 1     | 0       | 0        | 0            | 0                  | 0        |
| [High](#High)        | 3     | 0       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 2     | 0       | 0        | 0            | 0                  | 0        |
| [Low](#Low)          | 7     | 0       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope
The following smart contracts were in scope of the audit:

- [Presale.sol](https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol)
- [Airdropper.sol](https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol)

## Methodology

The auditing process pays special attention to the following considerations:
- Testing the smart contracts against both common and uncommon attack vectors.
- Assessing the codebase to ensure compliance with current best practices and industry standards.
- Ensuring contract logic meets the specifications and intentions of the client.
- Cross-referencing contract structure and implementation against similar smart contracts produced by industry leaders.
- Thorough line-by-line manual review of the entire codebase by community auditors.

## Vulnerability Classifications

| Vulnerability Level | Classification                                                                               |
|---------------------|----------------------------------------------------------------------------------------------|
| [Critical](#Critical)            | Easily exploitable by anyone, causing loss/manipulation of assets or data.                   |
| [High](#High)                | Arduously exploitable by a subset of addresses, causing loss/manipulation of assets or data. |
| [Medium](#Medium)              | Inherent risk of future exploits that may or may not impact the smart contract execution.    |
| [Low](#Low)                 | Minor deviation from best practices.                                                         |


# Protocol Graph (Optional)

IMAGE

# Findings & Resolutions

| ID      | Title                                                                                     | Category            | Severity | Status  |
|-------|-------------------------------------------------------------------------------------------|---------------------|----------|---------|
| [C-01](#C01)  | Owner can't get collected eth in Presale.sol                                      | Access Control        | Critical  | Pending |
| [H-01](#H01)  | Missing variable initialization on contract creation for Presale.sol              | Logic error           | HIGH      | Pending |
| [H-02](#H02)  | Presale.sol can't be compiled because of an incorrect variable access             | Logic error           | HIGH      | Pending |
| [H-03](#H03)  | airDropActive check in Presale.sol is wrongly implemented                         | Validation            | HIGH      | Pending |
| [H-04](#H04)  | Missing initialise for newToken in Airdropper.sol                                 | Logic error           | HIGH      | Pending |
| [M-01](#M01)  | Airdropper.sol can receive nativ tokens that will be stuck                        | Validation            | MEDIUM    | Pending |
| [M-02](#M02)  | Airdropper.sol implementation for airdropping is to error prune and centrelized   | Centralization risk   | MEDIUM    | Pending |
| [L-01](#L01)  | Cap check in Presale.sol can be gamed by an attacker                              | Validation            | LOW       | Pending |
| [L-02](#L02)  | Use specific imports instead of just a global import                              | Implementation        | LOW       | Pending |
| [L-03](#L03)  | Update pragma versioning to >= 0.8.16                                             | Implementation        | LOW       | Pending |
| [L-04](#L04)  | SafeMath / SafeMathInt not needed in solidity >= 0.8.x                            | Implementation        | LOW       | Pending |
| [L-05](#L05)  | Remove unused usings                                                              | Implementation        | LOW       | Pending |
| [L-06](#L06)  | Use safeTransfer instead of transfer                                              | Token integration     | LOW       | Pending |
| [L-07](#L07)  | Unused Airdropped event                                                           | Logic error           | LOW       | Pending |


## <a id="Critical"></a>Critical

### <a id="C01"></a> C-01 Owner can't get collected eth in Presale.sol

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L27-L75


#### Description:
Presale.sol is missing a function where the owner can withdraw the ETH users spent to buy tokens.


#### Recommendation:
Add a function with access controll for `onlyOwner` to withdraw the ETH in contract.


#### Resolution:


----


## <a id="High"></a> High

### <a id="H01"></a> H-01 Missing variable initialization on contract creation for Presale.sol

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L38


#### Description:
Users can buy presale tokens via a `ETH` transfer in `Presale.sol`, but the contract has an emtpy constructor and doesn't initialize the required variables `token`, `cap` and `exchangeRatio`. 
Due to this no one can buy tokens as the first require will never pass `require(boughtAmount[msg.sender].add(value) <= cap, "");` because cap = 0.


#### Recommendation:
Initialize `cap, token and exchangeRatio` in contstructor. Also add require checks for `exchangeRatio > 0`, `cap > 0` and `token != address(0)` when setting this variables.




#### Resolution:



### <a id="H02"></a> H-02 Presale.sol can't be compiled because of an incorrect variable access

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L52


#### Description:
The current implementation
```solidity
require(value == msg.value, "");
```
in `buyPresale` is not possible as the function is not payable and so it's not possible to access msg.value in a public function like that.

#### Recommendation:
Make the function payable or private (take care of [H-03](#H03) if making the function payable)




#### Resolution:



### <a id="H03"></a> H-03 airDropActive check in Presale.sol is wrongly implemented

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L41


#### Description:
Currently it's only checked in `receive` if airDropActive is true, but as the buyPresale is public this check can be bypassed if buyPresale is called directly.
As long as the contract has tokens or as soon as it gets new tokens an attacker can buy tokens, even if the airDrop is currently deactivated.

#### Recommendation:
Move the `require(airDropActive,"presale closed");` check to the `buyPresale` function or make buyPresale private.


#### Resolution:

### <a id="H04"></a> H-04 Missing initialize for newToken in Airdropper.sol

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L40


#### Description:
Currently the state variable `newToken` is not set in Airdropper and it's not possible to set it and so it will always be address(0).
Without a properly set newToken variable the complete contract is not usable and it's impossible for the owner to transfer the airdrop.

#### Recommendation:
Initialize variable on constructor and check for address(0).


#### Resolution:



-----------------



## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Airdropper.sol can receive nativ tokens that will be stuck

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L42


#### Description:
The Airdropper is responsible to airdrop the tokens and should never hold any nativ tokens. 
If a user transfers a native token to the contract it will not revert and the tokens will be stuck in the contract as there is no function to recover it.

#### Recommendation:
Remove `receive() external payable {}`


#### Resolution:


### <a id="M02"></a> M-02 Airdropper.sol implementation for airdropping is to error prune and centrelized

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L47-L56


#### Description:
The current implementation how tokens will be distributed is not good as there are too many errors that can happen and the distribution is solely done by the owner and he can do anything he wants. Users can't verify anything and can't claim airdrops by them selfe.

#### Recommendation:
You should refactore the airdrop to use a mercle tree for distribution. 
With a merce treee distribution, all users can verify the exact amount that will be distributed in total, they can claim it by them selfe and don't rely on the owner to make no errors.
The owner can still distribute the airdrop for the users if wanted, but the function `unlockAirdrop` and `emergencyWithdraw` can be removed as the total values the contract needs to hold will be predefined and it's impossible for an user to get the wrong amount of tokens.

#### Resolution:

-----------------


## <a id="Low"></a> Low

### <a id="L01"></a> L-01 Cap check in Presale.sol can be gamed by an attacker


https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L53

#### Description:
The current check `require(boughtAmount[msg.sender].add(value) <= cap, "");` can be gamed by an attacker and all tokens in Presale.sol could be bought from one user.
The current implementation only checks for msg.sender, it's possible for an attacker to use a contract to deploy a new contract via create and in the constructor of the deployed contract implement the buyPresale, buy maximum amount of tokens, transfer them to the attacker and repeat.

1. Deploy attacker contract
2. attacker contract deploy new contract in a loop
3. deployed contract in loop use buyPresale to buy maximum allowed `cap`
4. repeat until desired amount is reached or Presale contract is empty


#### Recommendation:
Consider another check like checking for a signed message that needs to be generated by the protocol. This is also attackable, but makes it a bit harder and not so automated.


#### Resolution:


### <a id="L02"></a> L-02 Use specific imports instead of just a global import

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L5-L8
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L5-L8

#### Description:
For a better better developer experience it's better to use specific imports instead of just a global import. This helps to have a better overview what is really needed and helps to have a clearer view of the contract.

```solidity
File: contracts/Airdropper.sol
File: contracts/Presale.sol
```

#### Recommendation:
```solidity
import {Ownable} from "./Ownable.sol";
```


#### Resolution:


### <a id="L03"></a> L-03 Update pragma versioning to >= 0.8.16

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L2
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L2

#### Description:
Consider a newer version >= 0.8.16 for your contracts - https://docs.soliditylang.org/en/v0.8.17/bugs.html

```solidity
File: contracts/Airdropper.sol
File: contracts/Presale.sol
```

#### Recommendation:
```solidity
pragma solidity 0.8.17;
```

#### Resolution:


### <a id="L04"></a> L-04 SafeMath / SafeMathInt not needed in solidity >= 0.8.x

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L28
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L28

#### Description:
Solidity 0.8.x has implemented over/underflow protection and so it's not needed to use the libraries.

#### Recommendation:
Remove `using SafeMath for uint256;` and use direct operations like * / + / .. instead of xx.mul(y)

#### Resolution:


### <a id="L05"></a> L-05 Remove unused usings

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L7
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L30
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L7
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L30

#### Description:
The import and using of Address is never used and should be removed.

```solidity
File: contracts/Airdropper.sol
07: import "./Address.sol";
30:     using Address for address; 

File: contracts/Presale.sol
07: import "./Address.sol";
30:     using Address for address;
```

#### Recommendation:
Remove the import and using lines.

#### Resolution:


### <a id="L06"></a> L-06 Use safeTransfer instead of transfer

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L67
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L56
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L68

#### Description:
You should use safeTransfer instead of transfer. As the token is deployed by the owner, it should revert on a failed transfer, but as some tokens don't revert it's best practice to use safeTransfer

```solidity
File: contracts/Presale.sol
56:        IERC20(token).transfer(msg.sender, value); 
68:        IERC20(token).transfer(payable(msg.sender), IERC20(token).balanceOf(address(this))); 

File: contracts/Airdropper.sol
67:        token.transfer(payable(msg.sender), token.balanceOf(address(this)));

```

#### Recommendation:
Change `transfer` to `safeTransfer`

#### Resolution:


### <a id="L07"></a> L-07 Unused Airdropped event

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L35-L38

#### Description:
Currently the Airdropped event is not used in Airdropper.sol, you should emit the event after setting isAirdropped[holders[i]] = true; or remove the event

#### Recommendation:
Emit event

```solidity
File: contracts/Airdropper.sol
53:                 isAirdropped[holders[i]] = true;
54:                 emit Airdropped(amounts[i], holders[i]);
```

#### Resolution:


____

## Disclaimer
> 
> This report is not, nor should be considered, an “endorsement” or “disapproval” of any particular project or team. This report is not, nor should be considered, an indication of the economics or value of any “product” or “asset” created by any team or project that contracts Solidity Lab to perform a security assessment. This report does not provide any warranty or guarantee regarding the absolute bug-free nature of the technology analyzed, nor do they provide any indication of the technologies proprietors, business, business model or legal compliance.
> 
> This report should not be used in any way to make decisions around investment or involvement with any particular project. This report in no way provides investment advice, nor should be leveraged as investment advice of any sort. This report represents an extensive assessing process intending to help our customers increase the quality of their code while reducing the high level of risk presented by cryptographic tokens and blockchain technology.
> 
> Blockchain technology and cryptographic assets present a high level of ongoing risk. Solidity Lab’s position is that each company and individual are responsible for their own due diligence and continuous security. Solidity Lab’s goal is to help reduce the attack vectors and the high level of variance associated with utilizing new and consistently changing technologies, and in no way claims any guarantee of security or functionality of the technology we agree to analyze.
> 
> The assessment services provided by Solidity Lab is subject to dependencies and under continuing
> development. You agree that your access and/or use, including but not limited to any services, reports, and materials, will be at your sole risk on an as-is, where-is, and as-available basis. Cryptographic tokens are emergent technologies and carry with them high levels of technical risk and uncertainty. The assessment reports could include false positives, false negatives, and other unpredictable results. The services may access, and depend upon, multiple layers of third-parties.
> 
> Notice that smart contracts deployed on the blockchain are not resistant from internal/external exploit. Notice that active smart contract owner privileges constitute an elevated impact to any smart contract’s safety and security. Therefore, Solidity Lab does not guarantee the explicit security of the audited smart contract, regardless of the verdict.

<br/>

____

## About

Solidity Lab is a community of practicing auditors guided by Guardian Audits.

To learn more, visit https://lab.guardianaudits.com

To view the Solidity Lab audit portfolio, visit https://github.com/GuardianAudits/LabAudits