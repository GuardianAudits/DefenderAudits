![](https://i.imgur.com/zGzXYfT.jpg)


__Audit Firm__: Solidity Lab

__Client Firm__: Poodl Tech

__Prepared By:__ Team 1

__Delivery Date:__ March 7th, 2023

__Auditors:__ [Shen](https://twitter.com/extcodehash), [cRat1st0s](), [Cryptanu](), [ZOUVIER](https://twitter.com/ZoumanaCisse6)

<br />

__PoodlTech__ engaged Solidity Lab to review the security of its Smart Contract system. From __February  25th, 2023__ to __March 7th, 2023__, a team of __4__ auditors reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | poodlTech                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | https://github.com/poodlTech/tokenAudit                             |
| Commit       | [eebe267b3fdd75a82e09cc270b3c046b2c9f2c84](https://github.com/poodlTech/tokenAudit/tree/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84) |


| Delivery Date     | March 7th, 2023       |
|-------------------|--------------------------------|
| Audit Methodology | Static Analysis, Manual Review |


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 0     | 0       | 0        | 0            | 0                  | 0        |
| [High](#High)        | 2     | 2       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 7     | 7       | 0        | 0            | 0                  | 0        |
| [Low](#Low)          | 11     | 11       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope

| ID File | SHA-256      | Checksum                              |
|---------|------------|---------------------------------------|
| A      | contracts/DividendPayingToken.sol |03754dd9bb6810afaeea9f89c43b6587a123bc485aa7383cebe1d89480fe7b12  |
| B      | contracts/Token.sol | 415199e53bce4ef12717900ca4750e5fbf53b3451f0e17702dae3f1a47ba6f76 |
| C      | contracts/Presale.sol | 15e348fa45d6d18fa032638b18e31ea0332532daadcab8aa610a42d6679fa01f |
| D     | contracts/Airdropper.sol | a3c1bfbf048d07dd257994ebf94b1984fd7160f5c0cbd579388da1cf13fc6389 |

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




# Findings & Resolutions

| ID      | Title                                                                                     | Category            | Severity | Status  |
|-------|-------------------------------------------------------------------------------------------|---------------------|----------|---------|
| [H-01](#C01)  |User can Withdraw more than owned|| HIGH | Pending |
| [H-02](#H01)  |Sandwichable functions|| HIGH     | Pending |
| [M-01](#M01)  |Reentrancy|| MEDIUM   | Pending |
| [M-02](#M02)  |Neglected return values|| MEDIUM   | Pending |
| [M-03](#M03)  |No address checks|| MEDIUM   | Pending |
| [M-04](#M04)  |Incorrect interface used|| MEDIUM   | Pending |
| [M-05](#M05)  |function will always Revert|| MEDIUM   | Pending |
| [M-06](#M06)  |Potential DOS|| MEDIUM   | Pending |
| [M-07](#M07)  |Loss of Ether possible|| MEDIUM   | Pending |
| [L-01](#L01)  |Contract won't compile|Compile | LOW      | Pending |
| [L-02](#L02)  |Dead Code|| LOW      | Pending |
| [L-03](#L03)  |CloseAirdrop lacking modifier|| LOW      | Pending |
| [L-04](#L04)  | uninitialized variables|| LOW      | Pending |
| [L-05](#L05)  | unused event|| LOW      | Pending |
| [L-06](#L06)  |Inconsistent library usage          || LOW      | Pending |
| [L-07](#L07)  |unchecked return values           || LOW      | Pending |
| [L-08](#L08)  |incorrect return values|| LOW      | Pending |
| [L-09](#L09)  |incorrect use of payable|| LOW      | Pending |
| [L-10](#L10)  |potential unbounded array|| LOW      | Pending |
| [L-11](#L11)  |Event provides mislead information|| LOW | Pending |





## <a id="High"></a> High

### <a id="H01"></a> H-01 User can Withdraw more than owned

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L256-L262
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L347-L348
https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L131
https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L243



#### Description:
A user can claim an unfair amount of ETH using `claim()` or `processAccount()` function.
The system has in place a logic to check whenever users can claim their prizes, in fact using the function `processDividendTracker()` it is possible to parse all the users list and let users claim their prize if eligible, through the function `canAutoClaim()`. However this logic is not present when a user wants to claim its price using claim() or processAccount() functions (Token.sol).
This create a flaw in the logic: an user can monitor the mempool for a call to `distributeDividend()` function, front-run it by buying token for ETH, wait for the other transaction to get mined, finally claim the prize in ETH for their inflate balance and sell the token right away, and sell back token for ETHER.
the function `accumulativeDividendOf()` is responsible to calculate the dividend of the user based on their balance, the `magnifiedDividendPerShare` parameter will be set once the `distributeDividend()` function is called. 

Steps to reproduce the issue:
1. wait for someone call `distributeDividend()` 
2. front-run it with a transaction that buys $token with ETH, by setting high gas fees.
3. within the backrun transaction claim ETH with `Token.claim()`
4. sell token for ETH 

POC:

https://gist.github.com/extcodehash/59c26f88b82041d8465407188c70fdb7

#### Recommendation:
Use `canAutoClaim()` to ensure a user is eligible, within Token’s `claim()` and `processAccount()`, also it is important to update `lastClaimTimes` correctly.

#### Resolution:



### <a id="H02"></a> H-02 Sandwichable functions

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/DividendPayingToken.sol#L169-L174
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L388-L394



#### Description:

Both `swapTokensForETH()` and `swapETHForTokens()` calls to Uniswap V2 router are sandwichable. A malicious user can exploit the high slippage set by those calls to sandwich the user’s transaction and make profit.


#### Recommendation:

Set slippage accordingly or calculate through Uniswap V2 functions.



#### Resolution:

-----------------



## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Reentrancy

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/DividendPayingToken.sol#L182

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L137


#### Description:

Reentrancy possible depending on stipend value.

POC: https://gist.github.com/zouvier/bb4c2908eaca84be2cbb0ffd9151ad4b

The PoC uses `withdrawDividend` function from DividendPayingToken to demonstrate the vulnerability. However it can be used also `_withdrawDividend()` directly, call claim or processDividendTracker within Token contract.



#### Recommendation:

Recommendation:
Set stipend around 2300, which should be the value of a transfer. 
Favour Pull over Push for receiving funds

### <a id="M02"></a> M-02 Neglected return values

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L56
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L68



#### Description:

The return values of the IERC20 transfer call are neglected. This means a failed transfer would go through and spend more gas to call `closeAirdrop()`



#### Recommendation:

Consider handling reverts or failed transfers early.



#### Resolution:



### <a id="M03"></a> M-03 No address checks

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L191
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L113
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L127
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L147
L160


#### Description:

The internal `_transfer` function does not have any checks for the `_to` address.

There could be input validation checks for stipend. If the stipend is set too low then the functions `_withdrawDividendOfUser( )` and `swapETHForTokens( )` could fail because of a low gas value set.

There are no checks for ‘holders’ individual address validity, the dead address can be passed in and ’amounts’ can be empty too.




#### Recommendation:

- Better check _to address for at least 0 address
- Set a reasonable amount of gas to call these functions without reverting.
- Have the data input as a mapping and perform input validation checks on the data passed in as well.




#### Resolution:

### <a id="M04"></a> M-04 Incorrect interface used

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L57


#### Description:
Uniswap interface is used, but the pancakeswap address is supplied.
This is the incorrect way to implement the pancakeswap address, see link below for the correct way, and is also implemented for the wrong chain.





#### Recommendation:

- Stick to a single chain and specify the correct addresses
- Follow official documentation to implement interfaces
https://docs.pancakeswap.finance/code/smart-contracts/pancakeswap-exchange/v2/router-v2#interface

#### Resolution:

### <a id="M05"></a> M-05 function will always Revert

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L192


#### Description:

`_transfer` will always revert



#### Recommendation:

Remove either the next lines of codes or remove the revert part because this break the functionality.



#### Resolution:

### <a id="M06"></a> M-06 Potential DOS

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L251


#### Description:

A malicious user can DOS `processDividendTracker()` calls or at least make them over expensive by transferring very low amounts of tokens to their smart contracts. In order to DoS or make user waste gas, those contracts need to implement the `receive()` function in a way that it always spend maximum gas provided to that call. 

Here is an explanation: https://swcregistry.io/docs/SWC-113

#### Recommendation:

Some mitigations are already in place e.g. `includeInDividends` and `excludeFromDividends`, also the global tracker for which indexes have been processed already. However this is not enough to stop a very motivated attacker. Consider to set proper access control

#### Resolution:


### <a id="M07"></a> M-07 Loss of Ether possible

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L42
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L65-L67



#### Description:
Ether sent to the contract can be lost forever. There’s a payable fall back, but no way to send Ether back in case of users accidentally sending Ether

 
a) Sending ERC20 doesn’t require a payable address

b)The ERC20 is held on a contract’s bytecode and is not tied to that contract's Ether balance.

c) This function will cause the owner to send the the entire unit value of ETH in their contract but for their ERC20.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i) I.e contract has 1 ETH deposited, but 200 of the ERC20
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;ii)Owner attempts to emergency withdraw.
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;iii) Owner is only able to withdraw 1 ERC20 token at a time

d) ETH funds sent to this contract are lost forever
&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;i)No way to recover the funds


#### Recommendation:

- Have a function to withdraw locked ether / tokens in contract.
- The receive( ) function does not tally with contract logic and can be removed.




#### Resolution:


-----------------


## <a id="Low"></a> Low


### <a id="L01"></a> L-01 Contract won't compile


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L51-L52

#### Description:
The `buyPresale` is a non payable function that attempts to use `msg.value`. The global variable unusable in `buyPresale` function.
a) `msg.value` is only available in payable functions
b) All calls will revert



#### Recommendation:

Declare `buyPresale()` as a payable function to compile contracts. To use `msg.value`, the function must be set as payable.




#### Resolution:

### <a id="L02"></a> L-02 Dead Code


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L10
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L27
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L27
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L10


#### Description:

Reentrancy is inherited in Airdropper and Presale but nonReentrant is never used in these two contracts. 



#### Recommendation:

Dead code, consider removing.



#### Resolution:

### <a id="L03"></a> L-03 CloseAirdrop lacking modifier


https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Presale.sol#L60

#### Description:
`closeAirdrop()` is declared as public and it does not have any modifier. A possible scenario can be:
Airdrop is finished and closed, Owner decides to reOpen it and before sending tokens to the contract a malicious actor can frontrun him and call `closeAirdrop()`



#### Recommendation:

#### Resolution:

### <a id="L04"></a> L-04 uninitialized variables


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L33-L36
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L33


#### Description:

The variables, `cap`, `token`, `newToken` and `exchangeRatio` are not initialized after declaration. This would make them retain their default values of 0 for uint, address(0) for address, and false for bool variables. 



#### Recommendation:


All of the Presale contract functionality is limited by this. Set them properly.



#### Resolution:



### <a id="L05"></a> L-05 unused event


https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Airdropper.sol#L35-L38

#### Description:

Airdropped event is unused.



#### Recommendation:

Remove it if not needed.



#### Resolution:

### <a id="L06"></a> L-06 Inconsistent library usage


https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L117

#### Description:

SafeMath’s div not used.




#### Recommendation:

Could use SafeMath’s .div instead of / for uniformity.



#### Resolution:

### <a id="L07"></a> L-07 unchecked return values


https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L256-L262

#### Description:
Unchecked return values




#### Recommendation:


Check return values


#### Resolution:

### <a id="L08"></a> L-08 incorrect return values


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L457
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L470


#### Description:

`getAccountDividendsInfo` and `getAccountDividendsInfoAtIndex` should return only dividends not all account data.




#### Recommendation:

Change name of those functions or return only dividends info.



#### Resolution:

### <a id="L09"></a> L-09 incorrect use of payable


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L347-L348
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L414
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L52


#### Description:

payable property is not needed when handling ERC20 tokens. 



#### Recommendation:

Remove where not needed.



#### Resolution:


### <a id="L10"></a> L-10 potential unbounded array


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L47-L63

#### Description:

The larger the array, the higher the gas cost. It’s also possible to run out of gas while looping through these arrays. Owner can shorten the array and split the transactions but would cost more in the long run.



#### Recommendation:

Favour Pull over Push for airdrop users. Have the users collect their airdrops instead of sending out all tokens to multiple users at once



#### Resolution:


### <a id="L11"></a> L-11 event provides mislead information

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L253

#### Description

The event ProcessedDividendTracker is using tx.origin as a parameter and this can mislead external users.

#### Recommendation

Use msg.sender instead.
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
