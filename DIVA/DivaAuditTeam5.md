![](https://i.imgur.com/BCKndbr.jpg)


__Audit Firm__: Solidity Lab

__Client Firm__: DIVA Protocol

__Prepared By:__ Team 5 - [TrungOre](https://twitter.com/Trungore), [Duc](https://twitter.com/duc_hph)

__Delivery Date:__ April 15th, 2023

<br />

__DIVA Protocol__ engaged Solidity Lab to review the security of its Smart Contract system. From __15/03__ to __15/04__, a team of __2__ auditors reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | RaisinLabs                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | https://github.com/GuardianAudits/DivaAudit                             |
| Commit       | [5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e](https://github.com/GuardianAudits/DivaAudit/commit/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e) |


| Delivery Date     | April 15th, 2022       |
|-------------------|--------------------------------|
| Audit Methodology | Static Analysis, Manual Review |


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 1     | 1       | 0        | 0            | 0                  | 0        |
| [High](#High)        | 3     | 3       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 2     | 2       | 0        | 0            | 0                  | 0        |
| [Low](#Low)          | 2     | 2       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope
The following smart contracts were in scope for the audit:
* [diva-contracts/contracts/*](https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts)
* [oracles/DIVAOracleTellor.sol](https://github.com/GuardianAudits/DivaAudit/blob/main/oracles/contracts/DIVAOracleTellor.sol)

(excluding [diva-contracts/contract/mocks](https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts/contracts/mocks) & [diva-contracts/contract/test](https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts/contracts/test))

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

| ID           | Title                                                                                                                                                        | Category          | Severity | Status  |
| ------------ | ------------------------------------------------------------------------------------------------------------------------------------------------------------ | ----------------- | -------- | ------- |
| [C-01](#C01) | Round-down calculation is used to calculate the `_collateralAmountRemovedNetMaker` which can be abused by taker to take all the removed liquidity from maker | Steal fund        | CRITICAL | Pending |
| [H-01](#H01) | Incorrect treasury is used for fee allocation when removing liquidity                                                                                        | Lose fee         | HIGH     | Pending |
| [H-02](#H02) | The position token (long/short token) can't be minted for the address(0)                                                                                     | Logic error       | HIGH     | Pending |
| [H-03](#H03) | `_createContingentPoolLib` is suspicious of the reorg attack                                                                                                 | Steal fund        | HIGH     | Pending |
| [M-01](#M01) | Transferring a zero value amount may revert when creating a pool                                                                                             | Token integration | MEDIUM   | Pending |
| [M-02](#M02) | Lack of support for Fee-on-Transfer tokens                                                                                                                   | Token integration | MEDIUM   | Pending |
| [L-01](#L01) | Redundant requirement when requiring the `collateralAmount > 1e6` when creating a pool.                                                                      | Validation        | LOW      | Pending |
| [L-01](#L02) | Redundant check `block.timestamp > submissionEndTime` | Validation | LOW | Pending |




## <a id="Critical"></a>Critical


### <a id="C01"></a> C-01 Round-down calculation is used to calculate the `_collateralAmountRemovedNetMaker` which can be abused by taker to take all the removed liquidity from maker

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibEIP712.sol#L855-L857

#### Description:
In the function `LibEIP712._fillOfferRemoveLiquidityLib()`, the variable `_collateralAmountRemovedNetMaker` is calculated using a round-down calculation. By splitting the `_offerRemoveLiquidity.makerCollateralAmount` into small enough parts, the taker can make the value of `_collateralAmountRemovedNetMaker` become 0, which means the maker gains nothing in every "remove liquidity" call.

For instance,
* `_offerRemoveLiquidity.positionTokenAmount = 100`
* ` _offerRemoveLiquidity.makerCollateralAmount = 10`

Calling `LibEIP712._fillOfferRemoveLiquidityLib()` with `_collateralAmountRemovedNet = 5` results in the returned amount for the maker being 5 * 10 / 100 = 0. The taker can repeat this process 20 times to steal all the returned amount of the maker.

#### Impact 
Taker can take all of the maker's returned collateral.

#### Recommendation:
Consider calculating the collateral returned for the taker first. The remaining amount will be returned to the maker.


#### Resolution: 



----


## <a id="High"></a> High

### <a id="H01"></a> H-01 Incorrect treasury is used for fee allocation when removing liquidity

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L780-L785

#### Description:
In the function LibDIVA._removeLiquidityLib(), the fee is allocated for LibDIVAStorage._governanceStorage().treasury instead of the current treasury which is fetched by calling LibDIVA._getCurrentTreasury(). This may be the new treasury that has not yet passed the activation time.

#### Impact 
Since the protocol is fully decentralized, with the owner determined through a voting mechanism, the previous owner may experience a significant loss due to incorrect fee allocation for a period of two days.


#### Recommendation:
Modify lines [780->785](https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L780-L785) as follows: 
```soldiity=
 _allocateFeeClaim(
    _removeLiquidityParams.poolId,
    _pool,
    _getCurrentTreasury(gs), /// change here 
    _protocolFee
);
```


#### Resolution: 

### <a id="H02"></a> H-02 The position token (long/short token) can't be minted for the address(0)

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L546-L553
https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L670-L677

#### Description:
Functions `createContingentPool` and `addLiquidity()` consider `longRecipient = 0x` or `shortRecipient = 0x` as valid inputs to enable conditional burn use cases as per the docs:
![](https://i.imgur.com/VhuWE18.png)
![](https://i.imgur.com/sXCHtzA.png)

Both functions use the `IPositionToken(_shortToken).mint()` function to mint long/short tokens for the recipient. However, the PositionToken contract inherits from [ERC20.sol](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/master/contracts/token/ERC20/ERC20.sol) in the OpenZeppelin library, which will [revert](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/cf86fd9962701396457e50ab0d6cc78aa29a5ebc/contracts/token/ERC20/ERC20.sol#L252) if the minted address is `address(0)`. This will break the intended behavior of the protocol.

#### Recommendation:
Consider minting for the `0xdead` instead of `0x` in case of conditional burn. 

#### Resolution: 

### <a id="H03"></a> H-03 `_createContingentPoolLib` is suspicious of the reorg attack

#### Description:
The `_createContingentPoolLib` function creates a new pool and determines their ids using the counter `ps.poolId`. At the same time, block reorg may happen on any blockchain, especially with some sidechain like Polygon, Optimism, ...
* https://polygonscan.com/blocks_forked

Note that, reorg on Polygon happens really often in which some are > 5 minutes long. For instance, there was an incident of 5.5 minites long reorg on Feb-22-2023 
* https://polygonscan.com/block/39599624/f?hash=0x0b7e6c5e9fbae3e2dbd114e4836b52ffb1211820bf62bbbd3ddf859dd07c0fe1
![](https://i.imgur.com/DAuEUYs.png)

With the block reorg, the attacker can execute some malicious actions to steal users's fund. Here is an example: 
* Alice creates a new pool, and some users add liquidity to it 
* Bob sees that the network block reorg happens and calls `_createContingentPoolLib()`. Thus, it creates a pool with an id to which some users add liquidity to. Then some users 's transactions are executed and they will transfer funds to Bob's controlled pool.
* Now Bob can
    * set his own dataProvider to control the output in the way that he can get the profit 
    * steal the tip which should be claimed by Alice's provider 

#### Attack scenario 
Before the reorg 
* Alice creates a pool with id = 1 
* Cindy add liquidity / add tip to the pool with id = 1 

After the reorg
* Bob creates a pool with id = 1 
* Alice creates a pool with id = 2 (which should be a pool with id = 1)
* Cindy add liquidity / add tip to the pool with id = 1 which is controlled by Bob 
--> Cindy's fund is at risk.

#### Recommendation:
Determine the pool id as hash of the `_createPoolParams` and the creator's address. 

#### Resolution: 

## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Transferring a zero value amount may revert when creating a pool 

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L476-L480
https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L645-L649

#### Description:
When working with ERC20 tokens, some tokens (such as LEND) [revert when transferring a zero value amount](https://github.com/d-xo/weird-erc20#revert-on-zero-value-transfers). However, the function `LibDIVA._createContingentPoolLib` always transfers an amount of collateral tokens equal to `_createPoolParams.collateralAmountMsgSender`, even when this value is equal to zero (indicating that msg.sender does not wish to transfer any tokens to create the pool). 

Note that there can be a situation when an user wants to make a new pool without transferring any tokens. It happens when a maker creates an EIP712 offer in which the taker will be the one who transfers all the funds to the pool (the amount that the taker transfers must be > 1e6). In this case, the create transaction will revert. 

#### Impact:
Users are unable to create a pool using certain collateral tokens without transferring any tokens.

#### Recommendation:
Just execute a transfer if `amount > 0`.

#### Resolution: 

### <a id="M02"></a> M-02 Lack of support for Fee-on-Transfer tokens 

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L475-L515

#### Description:There are ERC20 tokens that may make certain customizations to their ERC20 contracts.
One type of these tokens is deflationary tokens that charge a certain fee for every `transfer()` or `transferFrom()`. Note that these tokens doesn't violate the constraint of protocol's doc, they aren't rebasable: 
![](https://i.imgur.com/lTqhsik.png) 

Assume that the collateral token is a deflationary token, when a user transfers 10 tokens to the pool, he will get 10 shortToken + 10 longToken in return. When he try burn these 10 shortTokens + 10 longToken to get his tokens back, the transaction will revert. Because the actual amount that the pool receive is less than 10 tokens due to the deflationary mechanism. 

#### Recommendation:
Calculate the actual amount of receive token by calculating the difference of the token's balance before and after the transfer is excuted. 

#### Resolution: 

-----------------


## <a id="Low"></a> Low


### <a id="L01"></a> L-01 Redundant requirement when requiring the `collateralAmount > 1e6` when creating a pool. 

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/libraries/LibDIVA.sol#L602-L604

#### Description:
When creating a pool, the function `LibDiva._validateInputParamsCreateContingentPool()` forces the collateral amount must be larger or equal `1e6`. But this requirement can be bypass by withdrawing an amount tokens right after the creation. This make this requirement redundant.

#### Recommendation:
Remove the requirement if not necessary. 

#### Resolution: 

### <a id="L02"></a> L-02 Redundant check `block.timestamp > submissionEndTime`

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/facets/SettlementFacet.sol#L475

#### Description:
In the previous `if` block, it has a check whether `block.timestamp <= submissionEndTime`. So obviously in the `else` block, the `block.timestamp` is always larger than `submissionEndTime`, so the condition `block.timestamp > submissionEndTime` is redundant. 

#### Recommendation:
Remove the condition. 

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

To view the Solidity Lab audit portfolio, visit https://github.com/GuardianAudits/LabAudits![]
