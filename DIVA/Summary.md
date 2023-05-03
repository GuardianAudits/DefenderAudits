![](https://i.imgur.com/BCKndbr.jpg)


__Audit Firm__: Solidity Lab

__Client Firm__: DIVA Protocol

__Prepared By:__ Owen Thurm

<br />

__DIVA Protocol__ engaged Solidity Lab to review the security of its Smart Contract system. From __the 26th of March__ to __the 20th of April__, __4__ teams of __12__ total auditors reviewed the source code in scope. All Critical/High/Medium findings have been recorded in the following report, for Low/Informational/Gas findings refer to the individual team reports in this directory.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | DIVA                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | https://github.com/GuardianAudits/DivaAudit/tree/main/diva-contracts/contracts                             |
| Commit       | 5d0c7f6 |


| Delivery Date     | 04/20/2023       |
|-------------------|--------------------------------|
| Audit Methodology | Static Analysis, Manual Review |


| Vulnerability Level | Total | Resolved | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 0     | 0       | 0        | 0            | 0                  | 0        |
| [High](#High)        | 4     | 4       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 5     | 5       | 0        | 0            | 0                  | 0        |


# Audit Scope & Methodology

## Scope

| File                                                               | nSLOC    | Complex. Score |
| ------------------------------------------------------------------ | -------- | -------------- |
| oracles/contracts/DIVAOracleTellor.sol                             | 480      | 344            |
| diva-contracts/contracts/DIVADevelopmentFund.sol                   | 145      | 123            |
| diva-contracts/contracts/DIVAOwnershipMain.sol                     | 128      | 86             |
| diva-contracts/contracts/DIVAOwnershipSecondary.sol                | 114      | 35             |
| diva-contracts/contracts/DIVAToken.sol                             | 12       | 5              |
| diva-contracts/contracts/Diamond.sol                               | 118      | 117            |
| diva-contracts/contracts/PermissionedPositionToken.sol             | 58       | 41             |
| diva-contracts/contracts/PositionToken.sol                         | 39       | 28             |
| diva-contracts/contracts/PositionTokenFactory.sol                  | 57       | 44             |
| diva-contracts/contracts/UsingTellor.sol                           | 14       | 6              |
| diva-contracts/contracts/facets/ClaimFacet.sol                     | 69       | 44             |
| diva-contracts/contracts/facets/DiamondCutFacet.sol                | 10       | 7              |
| diva-contracts/contracts/facets/DiamondLoupeFacet.sol              | 43       | 36             |
| diva-contracts/contracts/facets/EIP712AddFacet.sol                 | 47       | 22             |
| diva-contracts/contracts/facets/EIP712CancelFacet.sol              | 77       | 58             |
| diva-contracts/contracts/facets/EIP712CreateFacet.sol              | 119      | 33             |
| diva-contracts/contracts/facets/EIP712RemoveFacet.sol              | 47       | 22             |
| diva-contracts/contracts/facets/GetterFacet.sol                    | 135      | 79             |
| diva-contracts/contracts/facets/GovernanceFacet.sol                | 253      | 105            |
| diva-contracts/contracts/facets/LiquidityFacet.sol                 | 89       | 47             |
| diva-contracts/contracts/facets/PoolFacet.sol                      | 35       | 33             |
| diva-contracts/contracts/facets/SettlementFacet.sol                | 328      | 131            |
| diva-contracts/contracts/facets/TipFacet.sol                       | 42       | 29             |
| diva-contracts/contracts/libraries/LibDIVA.sol                     | 499      | 196            |
| diva-contracts/contracts/libraries/LibDIVAStorage.sol              | 86       | 16             |
| diva-contracts/contracts/libraries/LibDiamond.sol                  | 240      | 116            |
| diva-contracts/contracts/libraries/LibDiamondStorage.sol           | 26       | 6              |
| diva-contracts/contracts/libraries/LibEIP712.sol                   | 558      | 733            |
| diva-contracts/contracts/libraries/LibEIP712Storage.sol            | 17       | 6              |
| diva-contracts/contracts/libraries/LibOwnership.sol                | 21       | 8              |
| diva-contracts/contracts/libraries/SafeDecimalMath.sol             | 14       | 7              |
| diva-contracts/contracts/interfaces/IClaim.sol                     | 25       | 9              |
| diva-contracts/contracts/interfaces/IDIVADevelopmentFund.sol       | 19       | 30             |
| diva-contracts/contracts/interfaces/IDIVAOwnershipMain.sol         | 27       | 33             |
| diva-contracts/contracts/interfaces/IDIVAOwnershipSecondary.sol    | 10       | 11             |
| diva-contracts/contracts/interfaces/IDIVAOwnershipShared.sol       | 3        | 3              |
| diva-contracts/contracts/interfaces/IDiamondCut.sol                | 14       | 3              |
| diva-contracts/contracts/interfaces/IDiamondLoupe.sol              | 7        | 9              |
| diva-contracts/contracts/interfaces/IEIP712Add.sol                 | 15       | 5              |
| diva-contracts/contracts/interfaces/IEIP712Cancel.sol              | 5        | 13             |
| diva-contracts/contracts/interfaces/IEIP712Create.sol              | 15       | 5              |
| diva-contracts/contracts/interfaces/IEIP712Remove.sol              | 15       | 5              |
| diva-contracts/contracts/interfaces/IERC165.sol                    | 3        | 3              |
| diva-contracts/contracts/interfaces/IGetter.sol                    | 5        | 45             |
| diva-contracts/contracts/interfaces/IGovernance.sol                | 90       | 21             |
| diva-contracts/contracts/interfaces/ILiquidity.sol                 | 31       | 9              |
| diva-contracts/contracts/interfaces/IPermissionedPositionToken.sol | 4        | 15             |
| diva-contracts/contracts/interfaces/IPool.sol                      | 11       | 5              |
| diva-contracts/contracts/interfaces/IPositionToken.sol             | 4        | 13             |
| diva-contracts/contracts/interfaces/IPositionTokenFactory.sol      | 3        | 5              |
| diva-contracts/contracts/interfaces/ISettlement.sol                | 53       | 13             |
| diva-contracts/contracts/interfaces/ITellor.sol                    | 3        | 3              |
| diva-contracts/contracts/interfaces/ITip.sol                       | 14       | 5              |
| **Totals**                                                         | **4296** | **2826**       |


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



| ID           | Title                                                                                            | Severity      | Status  |
| ------------ | ------------------------------------------------------------------------------------------------ | ------------- | ------- |
| [H-01](#H01) | Wrong implementation of EVMCall in DIVAOwnershipSecondary                                        | High          | Resolved |
| [H-02](#H02) | Funds could be stuck in DIVADevelopmentFund                                                      | High          | Resolved |
| [H-03](#H03) | Round-down calculation is used to calculate the `_collateralAmountRemovedNetMaker` which can be abused by taker to take all the removed liquidity from maker | Steal fund        | High | Resolved |
| [H-04](#H04) | `_createContingentPoolLib` is suspicious of the reorg attack                                                                                                 | Steal fund        | High     | Resolved |
| [M-01](#M01) | Wrong protocol fee recipient when withdrawing liquidity                                          | Medium        | Resolved |
| [M-02](#M02) | PreviousFallbackDataProvider won't have incentive to provide accurate value                      | Medium        | Resolved |
| [M-03](#M03) | Fee-on-Transfer tokens used as collateral will make a pool undercollateralized                   | Medium        | Resolved |
| [M-04](#M04) | DoS in `_calcPayoffs` function when calculating big numbers                                      | Medium        | Resolved |
| [M-05](#M05) | `_getActualTakerFillableAmount` Will Return `_takerCollateralAmount - _offerInfo.takerFilledAmount` Even If The Order Is Not Fillable | Logic | Medium | Resolved |


## <a id="High"></a> High

### <a id="H01"></a> H-01 Wrong implementation of EVMCall in DIVAOwnershipSecondary
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipSecondary.sol#L113

#### Summary
The implementation in `DIVAOwnershipSecondary.getQueryDataAndId()` is generating a wrong `queryId` as the `queryData` is wrongly encoded.

#### Description
Tellor is using the [Tellor Data Specifications](https://github.com/tellor-io/dataSpecs#create-new-query-type) as the standard for how to structure a query.

The [EVMCall](https://github.com/tellor-io/dataSpecs/blob/main/types/EVMCall.md) is used to query and validate data from one chain on another. 
> The `EVMCall` query type allows users to bridge on-chain data from one EVM chain to another. Users can tip Tellor reporters to bridge balances, NFT ownership, and any other public data from one EVM blockchain to another, even if there is no bridge.

The `EVMCall` query type parameters consits of the following: `chainId` + `contractAddress` and `calldata`. The calldata is the **queryData** used in `msg.data` to call the contract on a specific chain.

The current implementation is using the right function signature `0xa18a186b` for the call, but as it's wrongly encoded, the `queryData` will be wrong and the contract on mainnet can't be called with the calldata here.

Current implementation:
```bash
➜ uint256 _mainChainId = 1;
➜ address _ownershipContractMainChain = 0xe8a4517C0380A5aC4164B8e4C7A480E27E47830B;

➜ abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, 0xa18a186b))
0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000745564d43616c6c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000001000000000000000000000000e8a4517c0380a5ac4164b8e4c7a480e27e47830b00000000000000000000000000000000000000000000000000000000a18a186b

➜ keccak256(abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, 0xa18a186b)))
Type: bytes32
└ Data: 0xeb5360f9a90aa7690e0ac7e503d530c92aa954ae2e9b59f0db5fa6e1ed5a8888
```

Expected
```bash
➜ uint256 _mainChainId = 1;
➜ address _ownershipContractMainChain = 0xe8a4517C0380A5aC4164B8e4C7A480E27E47830B;

➜ abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, abi.encodeWithSignature("getCurrentOwner()")))
0x00000000000000000000000000000000000000000000000000000000000000400000000000000000000000000000000000000000000000000000000000000080000000000000000000000000000000000000000000000000000000000000000745564d43616c6c0000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000000a00000000000000000000000000000000000000000000000000000000000000001000000000000000000000000e8a4517c0380a5ac4164b8e4c7a480e27e47830b00000000000000000000000000000000000000000000000000000000000000600000000000000000000000000000000000000000000000000000000000000004a18a186b00000000000000000000000000000000000000000000000000000000

➜ keccak256(abi.encode("EVMCall", abi.encode(_mainChainId, _ownershipContractMainChain, abi.encodeWithSignature("getCurrentOwner()"))))
Type: bytes32
└ Data: 0x039f2dda5530f8d2778820316be007a497783803a1c5525828ad3aecc92b0212
```

The correct queryId should be `0x039f2dda5530f8d2778820316be007a497783803a1c5525828ad3aecc92b0212` but in the current implementation it is `0xeb5360f9a90aa7690e0ac7e503d530c92aa954ae2e9b59f0db5fa6e1ed5a8888`.

As of the wrong `queryData`, automated systems and reporters or the [monitoring](https://docs.tellor.io/tellor/disputing-data/monitoring) can't validate the `EVMCall`.

They will call `DIVAOwnershipMain` with a msg.data of `00000000000000000000000000000000000000000000000000000000a18a186b` instead of `a18a186b00000000000000000000000000000000000000000000000000000000`.

This results in calling the function with signature `00000000` instead of `a18a186b`. As this function signature doesn't exist in DIVAOwnershipMain the call will revert and reporters and validators can't validate the value.

#### Recommendation
Follow the Tellor Data Specifications for the EVMCall and change the function to the following:

```solidity
        queryData = 
                abi.encode(
                    "EVMCall",
                    abi.encode(_mainChainId, _ownershipContractMainChain, abi.encodeWithSignature("getCurrentOwner()"))
                );
```




-----------------

### <a id="H02"></a> H-02 Funds could be stuck in DIVADevelopmentFund
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#LL43C54-L43C54
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVADevelopmentFund.sol#L61

#### Summary
If a deposit  is done with _releasePeriodInSeconds = 0 all the tokens in that transaction will be stuck forever in the contract.

#### Description
In `DIVADevelopmentFund#deposit` when a user calls the deposit function with the `_releasePeriodInSeconds` set to zero, this would cause the token deposited following this call to be stuck in the contract forever. This is possible since the value of `_releasePeriodInSeconds` is not validated within the `deposit` function.
The reason for this behaviour is as a result of the check made in `DIVADevelopmentFund#withdraw`
```
if (_deposit.lastClaimedAt < _deposit.endTime) {//@audit-info trapping same block.timestamp event
    _claimableAmount += _claimableAmountForDeposit(_deposit);
    _deposit.lastClaimedAt = block.timestamp;
}
```
When `_releasePeriodInSeconds` is set to zero:
The `_deposits` array is updated with the following values:
```
_deposits.push(
            Deposit(
                _token,
                _amount,
                block.timestamp,
                block.timestamp + 0,
                block.timestamp
            )
        );
```
when a user(owner) tries to withdraw such deposit.
 _deposit.endTime equals to `block.timestamp + 0`, which means _deposit.lastClaimedAt == _deposit.endTime == _deposit.startTime. This means the condition (`if (_deposit.lastClaimedAt < _deposit.endTime)`) would never be met therefore no call would be made to `_claimableAmountForDeposit`. And the `_claimableAmount` for such deposit would not be accounted for.

For a scenario where a user supplies a very high value for the `_releasePeriodInSeconds` owner has to wait for that specified amount of time to be able to withdraw the token or for a reasonable _claimable amount to be calculated since all _claimableAmount would round to zero prior to the _deposit.endTime(or closer to that time). 

Note: tokens deposited with the `deposit` function can not be withdrawn using the `withdrawDirectDeposit` meaning if a vesting deposit can not be withdrawn using the `withdraw` function then the funds associated with such deposit are stuck forever. 

This issue is rated high since a user can consciously set `_releasePeriodInSeconds` to zero in an intention for the funds to be readily available for the DIVAdevelopment without the knowledge that the fund would be stuck forever.

#### Proof of Concept
see coded POC [here](https://gist.github.com/zaskoh/296a826902b2e9ceb0d2a8d7d5adcd57)

#### Recommendation

* check if the value of `_releasePeriodInSeconds` is zero and send the deposit directly to the contract, or change `<` to `<=` to the invariant on the withdraw function loop.

* The deposit should also check for reasonably maximum _releasePeriodInSeconds  like <= 30 years (as 30-year period was mentioned in the docs) to avoid funds getting stuck due to an unreasonable long _releasePeriodInSeconds


-----------------




### <a id="H03"></a> H-03 Round-down calculation is used to calculate the `_collateralAmountRemovedNetMaker` which can be abused by taker to take all the removed liquidity from maker

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


----



### <a id="H04"></a> H-04 `_createContingentPoolLib` is suspicious of the reorg attack

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


----------



## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Wrong protocol fee recipient when withdrawing liquidity
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L783


#### Summary
When withdrawing liquidity, the wrong fee recipient is used to allocate the protocol fees.

#### Description
In `LibDIVA` library, the `_removeLiquidityLib` function allocates the protocol fees to the wrong address. 

The current treasury address is determined by the following function:
```solidity
function _getCurrentTreasury(LibDIVAStorage.GovernanceStorage storage _gs)
    internal
    view
    returns (address)
{
    // Return the new treasury address if `block.timestamp` is at or past
    // the activation time, else return the current treasury address
    return
        block.timestamp < _gs.startTimeTreasury
            ? _gs.previousTreasury
            : _gs.treasury;
}
```
This means that if `startTimeTreasury` is bigger than `block.timestamp`, the treasury address is `preaviousTreasury` instead of `treasury`. Having established this, when the `_removeLiquidityLib` function allocates the protocol fees to the treasury address, it should call `getCurrentTreasury` to know what is the correct treasury address at that moment. Instead of that, is directly allocates the fees to `treasury`.

```solidity
// Allocate protocol fee to DIVA treasury. Fee is held within this
// contract and can be claimed via `claimFee` function.
// `collateralBalance` is reduced inside `_allocateFeeClaim`.
_allocateFeeClaim(
    _removeLiquidityParams.poolId,
    _pool,
    LibDIVAStorage._governanceStorage().treasury, // @audit-issue it should call `_getCurrentTreasury`
    _protocolFee
);
```

This means that when the owner changes the treasury address, it will start to receive fees from the withdrawal of liquidity at that exact time when in theory it should wait 2 days to update the treasury address. 

#### Recommendation
Change the affected code to call `_getCurrentTreasury` function to get the current treasury address instead of directly allocating the fees to `LibDIVAStorage._governanceStorage().treasury`.

```solidity

address _treasury = _getCurrentTreasury();

// Allocate protocol fee to DIVA treasury. Fee is held within this
// contract and can be claimed via `claimFee` function.
// `collateralBalance` is reduced inside `_allocateFeeClaim`.
_allocateFeeClaim(
    _removeLiquidityParams.poolId,
    _pool,
    _treasury, // @audit-ok
    _protocolFee
);
```




### <a id="M02"></a> M-02 PreviousFallbackDataProvider won't have incentive to provide accurate value
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/SettlementFacet.sol#L487

#### Summary
The `previousFallbackDataProvider` would have to submit value without a settlement fee.

#### Description
When there is change in the `fallbackDataProvider`, the new `fallbackDataProvider` has to wait for `startTimeFallbackDataProvider` to be able to submit a value but yet receives the SettlementFee within that period(60-day delay), meanwhile the `previousFallbackDataProvider` has to submit value as a result of the if condition [#L480](https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/SettlementFacet.sol#L480).
This is wrong since the `previousFallbackDataProvider` won't have incentive to provide an accurate value.  
```
if (msg.sender != LibDIVA._getCurrentFallbackDataProvider(_gs))
                    revert NotFallbackDataProvider();

_confirmFinalReferenceValue(
                    _poolId,
                    _pool,
                    _treasury,
                    _gs.fallbackDataProvider,//@audit-issue - new fallbackDataProvider receives settlement fee
                    _finalReferenceValue,
                    _gs
                );
```

#### Recommendation
change [SettlementFacet.sol#L487](https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/SettlementFacet.sol#L487) to:
```
_confirmFinalReferenceValue(
                    _poolId,
                    _pool,
                    _treasury,
                    msg.sender, //@audit-ok
                    _finalReferenceValue,
                    _gs
                );
```




### <a id="M03"></a> M-03 Fee-on-Transfer tokens used as collateral will make a pool undercollateralized
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L475-L490

#### Summary
Using fee-on-transfer tokens as collateral for a pool will make a pool undercollateralized thus not making users of that pool whole. 

#### Description
Fee-on-transfer tokens are those who apply a fee every time a transfer occurs. Actually there're some recognised tokens that use that mechanism (`STA`, `PAXG`) and some do not currently charge a fee but may do so in the future (e.g. `USDT`, `USDC`).

By using this kind of tokens as collateral for a pool, when transferring the tokens from the users to the pool, the contract will account for the whole amount of tokens but in reality there'll be less than accounted due to the fees. 

The documentation states that rebasable tokens shouldn't be used as collateral but it doesn't mention FoT tokens. 

Docs: 

![image](https://user-images.githubusercontent.com/90318659/227597356-1c5e4019-3e60-4e32-b837-11588959d4c4.png)

#### Recommendation
Describe explicitly in the documentation if fee-on-transfer tokens are allowed as collateral or implement a mechanism in the code to support this kind of tokens. 

Even if fee-on-transfer tokens are not supported, it's a best practice to change the code to check if the amount of `collateralToken` received is the same as expected before minting the S/L tokens. With this check, it's not possible to create a new pool or add liquidity to a pool if the fee-on-transfer was just activated. 




### <a id="M04"></a> M-04 DoS in `_calcPayoffs` function when calculating big numbers
https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L421-L430

#### Summary
The `_calcPayoffs` function reverts everytime is called with `_finalReferenceValue` >= `2e59` and `cap` > `_finalReferenceValue`.

#### Description
In `LibDIVA` library, the `_calcPayoffs` function calculates the final payout for the short and long token holders deResolved of the `finalReferenceValue` provided by the data provider. This function is called from `_setPayoutAmount`, that is called by `_confirmFinalReferenceValue` that is the function called when the data provider sets the final reference value. 

The issue is that when this function is called for a pool with a `finalReferenceValue` >= `2e59` and `cap` > `finalReferenceValue`, the function will always revert causing a DoS. 

**Example**
A group of people want to create a pool that needs a cap value of `1e60` or more because it needs a lot of precision (could be something science-related). In this case, when the expiry time of that pool arrives and the `finalReferenceValue` is close to `cap` the function will revert everytime leaving the users without their fair payout. 

If this scenario happens, they have 2 options:
 1. Wait for the submission period and review period to end and set the `inflection` as the final reference value.
 2. Wait for the submission period to end and the `fallbackDataProvider` can submit a `finalReferenceValue` that don't revert. 

Both options leaves the users receiving an unfair payout because the protocol didn't function as promised with large values. 

#### POC
https://gist.github.com/santipu03/ad81055d2698382fbf812315ef548ccb

#### Recommendation
The protocol has 2 options:
 - Before creating a pool, set a limit for the `cap` value so that this situation cannot happen. 
 - Warn users in the docuentation or website about the danger of setting large numbers in the pool. 


-----------------


### <a id="M05"></a> M-05 `_getActualTakerFillableAmount` Will Return `_takerCollateralAmount - _offerInfo.takerFilledAmount` Even If The Order Is Not Fillable

#### Description:

Since this https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibEIP712.sol#L555 is an internal function with little knowledge where it would be used , it is safe to say the output of this function is crucial.
Assume this scenario ->

1.) The `_makerCollateralAmount` is 0 (donation offer)

2.) `_offerInfo.status` is not OfferStatus.FILLABLE 

As according to the above two points , the status is not fillable so the function should be expected to return 0,
but it returns `_takerCollateralAmount - _offerInfo.takerFilledAmount` because , the function body starts executing and hits the first if condition , As prev mentioned the collateral amount specified is 0 as it is a donation request , so it enters the `if` body and satisfies the condition of the `if` and therefore returns `_takerCollateralAmount - _offerInfo.takerFilledAmount`

While it should have returned 0 (offer was not fillable).

#### Recommendation:

A possible solution can be 

```
if (_makerCollateralAmount == 0 && _offerInfo.status != OfferStatus.FILLABLE) {
            // Use case: donation request by maker
            return 0;
        }
```


-----------------


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

