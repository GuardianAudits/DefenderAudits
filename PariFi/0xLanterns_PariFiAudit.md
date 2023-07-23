![](https://hackmd.io/_uploads/Hyv4GdsYn.jpg)




__Audit Firm__: Guardian Defender

__Client Firm__: PariFi

__Prepared By:__ Kiki, Shealtielanz, Sword

__Delivery Date:__ July 20th, 2023

<br />

PariFi engaged Guardian Defender to review the security of its Smart Contract system. From June 28th to July 20th, a team of 3 auditors reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | PariFi                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | https://github.com/GuardianAudits/PariFiDefenderAudit/tree/main                               |
| Commit       | [835c98b](https://github.com/GuardianAudits/PariFiDefenderAudit/commit/835c98b66fecfe072750c400ba3ac1b0f6a07ebd) |


| Delivery Date     | July 20th, 2023       |
|-------------------|--------------------------------|
| Audit Methodology |Manual Review Static Analysis |


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 7     |   7     | 0        | 0            | 0                  | 0        |
| [High](#High)        | 7     | 7       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 16     | 16       | 0        | 0            | 0                  | 0        |
| [Low](#Low)          | 1     | 1       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope

| ID | File      | SHA-1 Hash                              |
|---------|------------|---------------------------------------|
| DF      | src/DataFabric.sol | 0c7dac9a47fbf5526afdb2fd81c43f9f777e22df |
| FM      | src/FeeManager.sol | a32a44376b3884793b312f6239c3318185b7e9a8 |
| MV      | src/MarketVault.sol | 0c101c6d286c18adf2a0016bef1174aebb3cbc4a |
| ODS     | src/OrderDS.sol | 5af392c2c678b7a1f5516bca240db986e7eb9bc0 |
| OM     | src/OrderManager.sol | d6543e7289a1553568063542085b487d96bd0e3b |
| PF     | src/ParifiForwarder.sol | 977d0cc719633bdf3acccd458f092f10e16ad2ae |
| PF     | src/PriceFeed.sol | f4e25a29b3489c4ef29ab648b521c8fa6903e103 |
| RB     | src/RBAC.sol | 3e20b89665c246b82030aadce9a495e8f6c4d222 |

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


# Inheritance Graph
![](https://i.imgur.com/Ke1VtTk.png)


# Findings & Resolutions

| ID      | Title                                                                                     | Category            | Severity | Status  |
|-------|-------------------------------------------------------------------------------------------|---------------------|----------|---------|
| [C-01](#C01)  | Liquidation threshold is calculated incorrectly.                                      | Logical Error                 | Critical     | Pending |
| [C-02](#C02)  | Users can avoid loosing collateral when at a loss by decreasing position.                                     | Logical Error                 | Critical     | Pending |
| [C-03](#C03)  | Impossible to liquidate when collateral is at zero.                                    | Logical Error                 | Critical     | Pending |
| [C-04](#C04)  | Users in profit will steal collateral from other users when closing position.                                     | Logical Error                 | Critical     | Pending |
| [C-05](#C05)  | Collateral does not update after fees are deducted.                                    | Logical Error                 | Critical     | Pending |
| [C-06](#C06)  | Losses get stuck in contract when position is closed.                                     | Logical Error                 | Critical     | Pending |
| [C-07](#C07)  | Premature  closing of position even when in safe range.                                     | Logical Error                 | Critical     | Pending |
| [H-01](#H01)  | Including fee in liquidation check casues premature liquidation.                                     | Logical Error                 | High     | Pending |
| [H-02](#H03)  | First depositor can casue subsequent users to loose funds.                                                        | Race Condition         | High     | Pending |
| [H-03](#H03)  | Risk free trade by sandwiching market orders.                                                        | Race Condition         | High     | Pending |
| [H-04](#H04)  | Attacker can frontrun fee distribution to steal yield from liquidity providers.                                                        | Race Condition         | High     | Pending |
| [H-05](#H05)  | Missing deadline parameter causes loss of funds to users/Traders         | Input validation     | High | Pending
| [H-06](#H06)  |  Incorrect Implementation of the `EIP-150` for meta transactions can lead to the Transactions always being `reverted` & `Gas grieving`| Logical Error         | High     | Pending |
| [H-07](#H07)  |  Malicious Actors can Grieve the relayers|  Logical error       | High     | Pending |
| [M-01](#M01)  | Frequent downtime with certain price feeds. | Logical Error   | Medium   | Pending |
| [M-02](#M02)  | Missing checks for Min and Max answer in price feed.                                                     | Output Validation      | Medium   | Pending |
| [M-03](#M03)  | Bad actor can grief honest user by preventing orders from being created.                                                     | Logic error         | Medium   | Pending |
| [M-04](#M04)  | Order can fail becasue fees are deducted between validation checks.                                                     | Logic error         | Medium   | Pending |
| [M-05](#M05)  | Users wont be able to take any profits if vault cant cover all profits.                                                     | Logic error         | Medium   | Pending |
| [M-06](#M06)  | Attacker can  sway price to the point of liquidation for users by manipulating  liquidity.                                                      | Logic error         | Medium   | Pending |
| [M-07](#M07)  | Users can close position when they should be liquidated to avoid liquidation fees.                                                     | Logic error         | Medium   | Pending |
| [M-08](#M08)  | Bad actor can grief keeper by increasing/decreasing position with dust amounts.                                                     | Logic error         | Medium   | Pending |
| [M-09](#M09)  | Missing logic to handle confidence interval for oracles.                                                  | Logic error         | Medium   | Pending |
| [M-10](#M10)  | Position cant close when fees are greater than collateral.                                                  | Logic error         | Medium   | Pending |
| [M-11](#M11)  | Users will be charged fees when market is closed.                                                      | Logic error         | Medium   | Pending |
| [M-12](#M12)  | Keepers can liquidate when protocol is paused.                                                   | Access Control         | Medium   | Pending |
| [M-13](#M13)  | Outdated `Pyth` prices used in `PariFinace`.                                                   | Logic Error         | Medium   | Pending |
| [M-14](#M14)  | Users who sign transactions are charged more `gas fees` than supposed to.                                                   | Logic Error         | Medium   | Pending |
[M-15](#M15)  | Missing `chainID` in the signature allows for `cross-chain` replay attacks.                                                   | Logic Error         | Medium   | Pending |
[M-16](#M16)  | Protocol Related Risk                                                  | Centralization Risk         | Medium   | Pending |
| [L-01](#L-01)  | `Eth` will get stuck in `Pricefeed.sol`.                                                   | Logic Error         | Medium   | Pending |


## <a id="Critical"></a>Critical


### <a id="C01"></a> C-01 | Liquidation threshold is calculated incorrectly.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L407-L410

#### Description:

Liquidation thresholds are used to determine if a position has taken on too much debt. Once they have crossed the threshold, they are eligible to be liquidated. According to the 'Arbitrum.sol' file, a common liquidation threshold is set at 80%. This means that if a position loses more than 20%, it can be liquidated. This is the norm across many DeFi platforms, so it is generally accepted.

However, there is an issue related to how the liquidation threshold is calculated and checked. Currently, it is calculated as follows: `uint256 liquidationThreshold = (market.liquidationThreshold * userPosition.collateralAmount) / PRECISION_MULTIPLIER;`. This calculation returns a value that is 80% of the collateral amount. Then, the check `if (lossInCollateral > liquidationThreshold)` is supposed to determine if the position has surpassed the liquidation threshold. However, what it is actually doing is checking if the losses are greater than 80% of the collateral. This means that instead of being liquidated when they lose more than 20%, they won't be liquidated until they lose 80% of the collateral. For example, a position with $100 in collateral won't be liquidated until the losses bring it down to $20 in collateral, when it should be liquidated at $80.


#### Recommendation:


Change the if statement to 
```solidity!
-- 410: if (lossInCollateral > liquidationThreshold)
++ 410: if (userPosition.collateralAmount - lossInCollateral < liquidationThreshold)
```


#### Resolution:


### <a id="C02"></a> C-02 | Users can avoid loosing collateral when at a loss by decreasing position.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L324-L331

#### Description:

When users are at a loss and need to close a position, the loss is deducted from their position, and the remaining collateral is returned to the user. However, users have found a way to avoid losing their collateral by decreasing their position before closing it. Currently, there is no check to see if a user is at a loss when decreasing their position. The only requirement is that the position's leverage is valid after the decrease.

As a result, users can decrease their position to a point where there is no loss, and the amount they decreased the position by will be returned to them. Then, when they close the position, the remaining collateral will also be sent back to them. This allows them to bypass any loss to their collateral that they should have incurred.

This loophole allows users to protect their collateral by strategically decreasing their position before closing it, effectively avoiding any losses they would have otherwise experienced.



#### Recommendation:

Deduct losses from positon before decreasing position. 



#### Resolution:


### <a id="C03"></a> C-03 | Premature closing of position even when in safe range.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L424

#### Description:

If a position is at a loss but above the liquidation threshold keepers can close their postion by calling `liquidatePosition`. At the end of the internal function `_liquidatePosition` the position gets deleted ` delete pendingOrders[_orderId];`. This is regardless and the only way the function will revert is if they are in profit.

So even if a user is at a small loss and should not be liqudated they can be and when that happens whatever loss + fees they fave will be deducted from the collateral 
```solidity
 if (userPosition.collateralAmount > lossInCollateral) {
            userBalance = userPosition.collateralAmount - lossInCollateral;
            IERC20(market.depositToken).safeTransfer(userPosition.userAddress, userBalance);
        }
```
And then the position will be deleted 
```solidity
delete openPositions[_positionId];

```
The user will loose some collateral + fees and have the position closed early. That collateral that they lost will also be locked in the contract forever. 


#### Recommendation:

Only delete the position when the postion has actually been liquidated. 



#### Resolution:



### <a id="C04"></a> C-04 | Users in profit will steal collateral from other users when closing position. 
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L247-L257

#### Description:

In this if-statement a user in profit will get the profits from the vault 
```solidity
    if (isProfit) {    
        userBalance = userPosition.collateralAmount + pnlInCollateral; 
        IMarketVault(market.marketAddress).withdrawUserProfits(pnlInCollateral);
```
And here the user will get an ammount equal to `userBalance`.
```solidity
     if (userBalance != 0) {
        IERC20(market.depositToken).safeTransfer(userPosition.userAddress, userBalance);
    }
```
The problem here is that the user is essentially getting their profits twice. Once from the vault, and another time in the form of collateral. So if a user has 100 dai in collateral and is 20 dai in proift. they should be getting 120 dai in total. 

But what ends up happening is they get 20 dai from the vault,  userBalance at this point is 120. So they also get 120 in collateral from this contract. This is a total of 140 dai. 20 of which is someone elses collateral. 





#### Recommendation:

After profits are taken form the vault deduct that amount from userBalance 
```solidity
    if (isProfit) {    
        userBalance = userPosition.collateralAmount + pnlInCollateral; 
        IMarketVault(market.marketAddress).withdrawUserProfits(pnlInCollateral);
++      userBalance = userBalance - pnlInCollateral;
```


#### Resolution:


### <a id="C05"></a> C-05 | Collateral does not update after fees are deducted. 

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L457
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L574-L577

#### Description:


When a user creates a position, they have to pay a fee, and those fees are taken out of the collateral and sent to the Fee Manager. So if a user creates a position with 100 DAI as collateral, 10 DAI can get taken out as fees. Meaning the user only has 90 DAI of collateral in the contract. However, the user's collateral does not get updated when the fees are deducted, so even though a user has 90 DAI collateral in the contract, it states that 100 DAI is actually stored.

A user can take advantage of this by canceling their order right after they create it, and the 100 DAI will be sent to them instead of the 90 DAI. The user breaks even in this scenario but ends up stealing collateral from other users. This can be repeated until the contract is drained, preventing core functions in the protocol from working (liquidation, close, decrease).

#### Recommendation:

Update the orders collateral amount to reflect the fees deducted.



#### Resolution:


### <a id="C06"></a> C-06 | Losses get stuck in contract when position is closed.  

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L252-L257

#### Description:

When a user closes a position at a loss, the loss will correctly be deducted from the user's position before transferring back any collateral. However, the loss does not get transferred to the Fee Manager and ends up staying in the contract unaccounted for. As a result, it remains locked in the contract permanently. This is an issue because those funds are owed to the liquidity providers.



#### Recommendation:

After the loss has been deducted from the users collateral, transfer that amount to the fee manager. 



#### Resolution:


### <a id="C07"></a> C-07 | Impossible to liquidate when collateral is at zero.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L404-L410

#### Description:

Note: Once `C-03` is fixed this vulnerability will present itself. 

Because of this if-statement whenever collateralAmount is 0 lossInCollateral will be as well. 

```solidity
      if (lossInCollateral > userPosition.collateralAmount) {
            lossInCollateral = userPosition.collateralAmount;
        }
```
Becasue of this calculation liquidationThreshold will also be 0 when collateral is 0.

```solidity
    uint256 liquidationThreshold =
            (market.liquidationThreshold * userPosition.collateralAmount) /                     PRECISION_MULTIPLIER;
```
If both values are zero than this if-statement will never be true. Meaning it will never enter a liquidatable state and wont be able to be liquidated.
```solidity
     if (lossInCollateral > liquidationThreshold) 
```

This gives the user unlimited upside with no risk of liqudation. 


#### Recommendation:

Add a check after the `isProfit` check. If the positon is at any loss and collateral is zero delete the position.

```solidity 
        if (isProfit) revert LibError.LiquidationErrorNoLoss();
if (userPosition.collateralAmount == 0) { delete openPositions[_positionId]}
```




#### Resolution:





----


## <a id="High"></a> High

### <a id="H01"></a> H-01 | Including fee in liquidation check casues premature liquidation. 

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L402

#### Description:

Currently the liquidation fee and closing fees are included in checking if a position is liquidated. 

Getting the total loss (including fees).
```solidity
        // Calculate fees - Liquidation fee + closing fee + accrued borrow fees
        uint256 totalFeesMarket = (market.liquidationFee + market.closingFee) * userPosition.positionSize / MAX_FEE;
        totalFeesMarket = totalFeesMarket + getAccruedBorrowFeesInMarket(userPosition.positionId);

        uint256 feesInCollateral = priceFeed.convertMarketToToken(market.marketId, totalFeesMarket, market.depositToken);
        uint256 lossInCollateral = pnlInCollateral + feesInCollateral;

```
Checking if loss passes the threshold.
```solidity
  if (lossInCollateral > liquidationThreshold)
```
This means that a user can be close to liquidation but not past the threshold, but becasue fees are added to `lossInCollateral` it will push them past the threshold and allow them to be liquidated. 


#### Recommendation:

Don't include liquidation fee and closing fee when checking if `lossInCollateral > liquidationThreshold`.




#### Resolution:


### <a id="H02"></a> H-02 | First depositor can casue subsequent users to loose funds.   

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L104

#### Description:

In the market vault, the first user to deposit will end up minting 10,000 shares, and those shares will be sent to a dead address. This measure is implemented to mitigate a first depositor attack. However, the deposit function is still missing the check that ensures the return value is not equal to 0.

As a result, a first depositor attack is still possible. Although the current mitigations make it more expensive and reduce the ROI for the attacker, as long as the return value can be 0, the attack remains possible.



#### Recommendation:

add
```solidity
++112     if(shares == 0) revert();
```



#### Resolution:


#### Resolution:
### <a id="H04"></a> H-03 | Risk free trade by sandwiching market orders.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L520

#### Description:

Because the amount of liquidity in a market influences the price, an attacker can exploit this by observing another user's market order and front-running that order with their own. By doing so, they can obtain a better price. Once the other user's order is executed, the attacker can then back-run the transaction and sell their position for a price higher than what they bought it for.

Since the front-run and back-run transactions occur in the same block, the price returned from the oracle will not be any different. However, the price determined by the market will change in favor of the attacker.

By capitalizing on the deterministic price increases, attackers can execute risk-free trades at the expense of the liquidity providers.


#### Recommendation:

Dont allow a user to open and close a position in the same block. If the transactions happen in differnt blocks this introduces risk to the attacker that the price could change in a unfavorable way. 



#### Resolution:
### <a id="H04"></a> H-04 |  Attacker can frontrun fee distribution to steal yield from liquidity providers.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L97

#### Description:

All profits that the liquidity providers receive occur in hour intervals. An attacker can take advantage of this fact by depositing a large sum of tokens into the `MarketVault` right before the fees are distributed to the liquidity providers. This way, the attacker will get a large portion of the fees. Immediately after the fees are distributed, the attacker can withdraw their principal + yield.

By doing this, the attacker is not exposing themselves to any of the risk that other liquidity providers are, and the attacker is getting an unfair share of the profits.

Because the fees are distributed every hour, an attacker can repeat this attack and constantly steal yield at no risk.

#### Recommendation:

2 options:

1) Add a timelock to the vault so that users must provide liquidity for a minimum amount of time before they can withdraw.

2) Remove the timelock from `distributeFees` this way the yield recieved by the LPs will be in small amounts making the attack unprofitable. 



### <a id="H05"></a> H-05 | Missing deadline parameter in a transaction causes loss of funds to users/Traders.
#### Lines of Reference.
- https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L22C1-L29C6
- https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L141C1-L153C30
#### Description.
As per the `docs`, It was noted in the area `5.2` ~  `Meta Transactions (Gasless
transactions)` that --->
> Users are also able to set a deadline, after which
a transaction cannot be executed, adding an extra
layer of control and flexibility to the trading process


But it wasn't implemented in the protocol. However, another issue here is `Signatures` signed by users should always have an `expiration` or `timestamp deadline`, such that after that time the `signature` is no longer valid. If there is no signature expiration, a user by signing a message is effectively granting a "lifetime license" signature implementations should always include an expiration timestamp and aim to conform to EIP-712.
You can see here that the `verify()` function, doesn't check for a deadline and the transaction struct doesn't allow users specify a deadline.
```solidity
    function verify(Transaction calldata transaction, bytes calldata signature) public view returns (bool) {
        if (_nonces[transaction.fromAddress] != transaction.userNonce) revert LibError.InvalidNonce();

        address signer = _hashTypedDataV4(
            keccak256(
                abi.encode(
                    _TYPEHASH,
                    transaction.fromAddress,
                    transaction.toAddress,
                    transaction.txValue,
                    transaction.minGas,
                    transaction.userNonce,
                    keccak256(transaction.txData)
                )
            )
        ).recover(signature);
```
You can see the `transaction struct` doesn't contain a parameter to enable users' set a `deadline`.
```solidity
    struct Transaction {
        address fromAddress;
        address toAddress;
        uint256 txValue;
        uint256 minGas;
        uint256 userNonce;
        bytes txData;
    }
```
Traders sign this transactions in order to place orders, a situation where their orders can be placed during times unprofitable or unwanted by them, they will lose funds and would `dis-incentivize` people from using PariFinance.

#### Reccomendation.
Allow users to specify a `deadline`, and ensure the deadline is checked during `verification`.




### <a id="H06"></a> H-06 | Incorrect Implementation of the `EIP-150` for meta transactions can lead to the Transactions always being `reverted` & `Gas grieving`.
#### Lines of Reference.
- https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L183C1-L198C10
#### Description.
The issue is in the `PariFiForwarder.sol`, where `users/traders` can sign transactions to be executed by `relayers or Partners`, In the `execute()` function here the check to ensure that enough gas was forwarded by relayers to avoid gas griefing attacks where trader's transactions fail to be executed due to insufficient gas but due the 1/64 gas rule the function call goes through successfully and the relayers earn rewards while the trader's transaction wasn't completely executed due to the insufficient gas sent via the `external call`.
```solidity
        (bool success, bytes memory returndata) = transaction.toAddress.call{
            gas: transaction.minGas,
            value: transaction.txValue
        }(abi.encodePacked(transaction.txData, transaction.fromAddress));

        // Validate that the relayer has sent enough gas for the call.
        // See https://ronan.eth.limo/blog/ethereum-gas-dangers/
        if (gasleft() <= transaction.minGas / 63) {
            // We explicitly trigger invalid opcode to consume all gas and bubble-up the effects, since
            // neither revert or assert consume all gas since Solidity 0.8.0
            // https://docs.soliditylang.org/en/v0.8.0/control-structures.html#panic-via-assert-and-error-via-require
            /// @solidity memory-safe-assembly
            assembly {
                invalid()
            }
        }
```
here you can see the `check` to ensure that the relayers sent enough gas was set after the  external call has been made, and it uses an inline assembly `opcode` that consumes all the gas if enough gas wasn't given to the external call by the relayer.

There are `2` ways to ensure that the relayers sent enough gas for the transaction to be successful without any Out of Gas issues following the [`EIP-150`](https://eips.ethereum.org/EIPS/eip-150) standard.

- `1` --> to check that the relayer has sent enough gas before the `external call` is made, if not the execute() function fails to execute and sends back all the remaining `gas` refunding the `relayer` back his gas.
- `2` --> to check after the `external call`. Here, after the external call is made, it then checks to ensure that the `gasleft()` is greater than `transaction.mingas/63`, this way if the `gasleft` is less than or equal to `transaction.mingas/63` it triggers an inline assembly opcode(`invalid()`) that literally consumes all the available gas and forcefully reverts the transaction. `NOTE` --> Here the relayers do not get refunded their gas because the function is forced to fail with the [`invalid()`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L196C17-L196C26) opcode which forcefully reverts the transaction by consuming all the gas in the contract as of the function `call`.

In `PariFiForwarder.sol`, it uses `2`, where the check is done after and I would explain how this can be detrimental to the protocol.

There are already existing issues with both of the methods that follow the [`EIP-150 standard`](https://github.com/ethereum/EIPs/issues/1930).
However, where the check is done after doesn't truly ensure the transaction was done with enough gas because although it ensures that the external call was given enough gas and forces the function call to fail for that by ensuring it will revert, the check can still be made to pass even tho the external call was given less gas but fails due to another reason(where the external call reverted or succeeded early, and the callee returns any remaining gas back to the caller as per the `EIP-150 standard`) so that the `gasleft()` after the call is actually greater than `transaction.mingas/63`.

**I want to lay down two issues to have in mind before I continue.**

*According to the implementation in `PariFiForwarder.sol`:*
- If the `external call` wasn't sent enough data, all the gas is consumed due to the use of the `invalid()` opcode.
- The condition that the check ensures can be still broken even though the `external call` was sent enough gas by the `relayer`. How? well remember that if the external call goes through successfully, it returns 2 things that are to be returned by the `execute()` function and thats the [`(bool success, bytes memory returndata)`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L183C9-L183C48), and this two variable consumes gas as they are stored in memory when returned from the `external call`, in a situation where the `returndata`s data consumes a lot of gas due to its `length/size`, the `gasleft()` will be made to be less than or equal to the `transaction.mingas/63` thereby failing the check and triggering the inline assembly `invalid()` opcode, which will then force the current call to revert not minding if the external call was successful. Here you can see the Issues that rises already --->
- `1` ---> Here the relayer has lost money for his gas, although the external call was successful meaning the relayer doesn't get rewarded for the meta transaction even tho it was successful.
- `2` ---> The trader who signs the transaction doesn't get charged for gas fees although their transaction was successful, this is because the [`_chargeGasFees()`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L200) comes after that check is made.
- `3` ---> In a situation where the relayer wasn't aware he sent insufficient gas and the check fails, all the gas is consumed and the relayer doesn't get any gas refunded, thereby losing his/her money.

#### Reccomendation.
I recommend that since the developers want to ensure that the `relayer` has sent enough gas then the best way to go about it is ---> that the check should come before the external call is made in order to protect both the `relayer`s and the `users` from being grieved, while also ensuring that enough gas is sent to the external call as requested by the trader who signed the transaction with a desired `transaction.mingas`.
The `execute()` function should be re-written like this:
```solidity
    function execute(Transaction calldata transaction, bytes calldata signature, address feeToken)
        external
        payable
        returns (bool, bytes memory)
    {
        uint256 gasSent = gasleft();

        // Only allow tx calls to OrderManager and MarketVault contract
        if (transaction.toAddress != orderManager && !vaultsAllowed[transaction.toAddress]) {
            revert LibError.InvalidToAddress();
        }

        if (transaction.txValue != msg.value) revert LibError.InvalidTxValue();
        if (!verify(transaction, signature)) revert LibError.InvalidSignature();

        // Increase user tx nonce
       _nonces[transaction.fromAddress] = transaction.userNonce + 1;
         // @audit check that the relayer has sent enough gas
        require(gasleft() - gasleft() / 64  >= transaction.minGas, "not enough gas provided")

        (bool success, bytes memory returndata) = transaction.toAddress.call{
            gas: transaction.minGas,
            value: transaction.txValue
        }(abi.encodePacked(transaction.txData, transaction.fromAddress));

        _chargeGasFees(feeToken, transaction.fromAddress, gasSent);
        return (success, returndata);
    }

```
This way it ensures that the relayer has sent enough gas, and if not their gas is refunded back to them, where they are now secured as the `execute()` function ensures that enough gas is sent rather consuming their entire gas if they didn't send enough gas, and losing the gas even when the transaction was was successful but the `returndata` consumes some of the `gasleft()` making the check done afterward to fail thereby forcing the transaction to revert even though the `external call` was actually successful.



### <a id="H07"></a> H-07 | Malicious Actors can Grieve the relayers.
#### Lines of Reference.
- https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L183
#### Description.
The `execute()` function of the `PariFiForwarder.sol`, allows a relayers to execute meta transactions for traders who sign transactions. when it makes an external call to the `transaction.address` specified by the trader who sign the transaction, however it uses the `call` opcode and returns a boolean for `success` and a byte for the `return data`, however storing the `returndata` gotten from a `call` can lead to `gas grieving`, where the target contract to be called can be able to force this `call` to run out of gas via a `returndata bomb`. 
```solidity
    function execute(Transaction calldata transaction, bytes calldata signature, address feeToken)
        external
        payable
        returns (bool, bytes memory)
    {
        uint256 gasSent = gasleft();


        // Only allow tx calls to OrderManager and MarketVault contract
        if (transaction.toAddress != orderManager && !vaultsAllowed[transaction.toAddress]) {
            revert LibError.InvalidToAddress();
        }


        if (transaction.txValue != msg.value) revert LibError.InvalidTxValue();
        if (!verify(transaction, signature)) revert LibError.InvalidSignature();


        // Increase user tx nonce
        _nonces[transaction.fromAddress] = transaction.userNonce + 1;


        (bool success, bytes memory returndata) = transaction.toAddress.call{
            gas: transaction.minGas,
            value: transaction.txValue
        }(abi.encodePacked(transaction.txData, transaction.fromAddress));
```
A situation where the call can be forced to run out of data, Malicious actor can grief the `relayers` where, the target contracts address the sign in their `transaction` will force the `call` to run out of gas in order to make the transaction revert, they wouldn't gain anything but that would cost funds for the relayers who try executing their transactions, where the loss money for the `transaction's gas`, and also because the `transaction.from` is only charged gas fees only after the `call` is done successfully, meaning the malicous actors would never be charged for the gas fee because the `_chargeGasFees()` would never be called.

#### Reccomendation.
Make use of a `safeCall` instead of `call` where the returndata is left out in order to avoid calls running out of data due to a `returndata bomb`(lenghty byte)
This is how the `safeCall` should be implemented. I got a library that can be used ---> [`Link here`](https://github.com/sherlock-audit/2023-01-optimism/blob/main/optimism/packages/contracts-bedrock/contracts/libraries/SafeCall.sol#L17-L36) This library is `100%` safe as it was use by the `Optimism` Protocol.
```solidity
    function call(
        address _target,
        uint256 _gas,
        uint256 _value,
        bytes memory _calldata
    ) internal returns (bool) {
        bool _success;
        assembly {
            _success := call(
                _gas, // gas
                _target, // recipient
                _value, // ether value
                add(_calldata, 0x20), // inloc
                mload(_calldata), // inlen
                0, // outloc
                0 // outlen
            )
        }
        return _success;
    }
```

#### Resolution:


-----------------

## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 | Frequent downtime with certain price feeds.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L52

#### Description:


If prices from chainlink have not been updated for 31 minutes or longer any function that interacts with chainlink will revert. 

This is by design as seen here 
```solidity
    uint256 internal constant MAX_PRICE_DELAY_SECONDARY = 31 minutes;
```

However, most price feeds have a heartbeat (time between updates) that is much longer than 31 minutes. Many have 24-hour heartbeats. Chainlink will update the price if the deviation exceeds a certain threshold (e.g., 1.0%). However, if the price remains within that threshold, it will not update until the next heartbeat.

As a result, functions that call `_getChainlinkPrice()` often revert, causing frequent and extended periods of downtime for the protocol.


#### Recommendation:

Remove `MAX_PRICE_DELAY_SECONDARY` from the staleness check. 

The discrepency between this variable and chainlinks heartbeats will lead to conflict.



#### Resolution:


### <a id="M02"></a> M-02 | Missing checks for Min and Max answer in price feed.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L150

#### Description:


Chainlink has a safeguard in place where, if the price drops drastically, it will return a fixed `minAnswer`. Similarly, if the price rises significantly, it will return a fixed `maxAnswer`. When this occurs, there will be a discrepancy between the value returned by Chainlink and the actual value of the asset. For example, the returned price might be $2.50, while the actual price is $2.20.

If such a scenario were to happen in Parifi, it would have an impact on liquidations, opening positions, and other functionalities within the protocol.


#### Recommendation:

Add a check that if the price is equal to min or max revert instead of returning a innacurate price. 




#### Resolution:

### <a id="M03"></a> M-03 | Bad actor can grief honest user by preventing orders from being created.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L521
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L525

#### Description:

Because orderId is a value chosen by the user, a bad actor can engage in griefing by front-running the user's transaction with their own and using the same orderId.

As a result, the user's transaction will revert. This can be particularly detrimental if the user is making a time-sensitive order, such as increasing collateral to avoid liquidation or attempting to close a position while still in profit.




#### Recommendation:

Use a nonce for the order ID this way the id is out of the users hands, eliminating the possibility of a griefing atttack. 




#### Resolution:

### <a id="M04"></a> M-04 |  Order can fail becasue fees are deducted between validation checks. 

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L446
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L457
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L209

#### Description:


Currently `validateLeverage()` is called twice when creating a position. Once at the begining of `_createOrderNewPosition()`. And again in `_createNewPosition()`. The issue is that the leverage ratio changed between these two checks due to fees being deducted here in `_createOrderNewPosition()`.

```solidity
    IERC20(market.depositToken).safeTransfer(feeManager, feeInCollateral);

```

This means that a user can create a position that is valid when created but not when settled by the keeper. This would be especially true if a user took out a maximum leverage position. 

When C-05 is fixed this vulnerability will cause issues.



#### Recommendation:

Add another `validateLeverge()` in `_createOrderNewPosition()` here:
```solidity
++ 458:   _validateLeverage(bytes32(0), market.marketId, _order.orderSize, _order.collateralAmount);

```




#### Resolution:

### <a id="M05"></a> M-05 |  Users wont be able to take any profits if vault cant cover all profits. 

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L249

#### Description:


Currently, there is no way for a user to take partial profits. If a user has more profit than there are funds in the vault, they will not be able to close their position.

Due to this limitation, the user's only options are to either decrease the position (sacrificing some profit) or wait for the price to move in an unfavorable way, resulting in a lower value for their position.

### Recommendation:

To address this issue, it is suggested to either allow users to take partial profits or enable them to close a position and lock in the profits. The latter can be achieved by crediting the owed profit to the user, allowing them to claim it at a later time when the vault has sufficient funds.


#### Resolution:

### <a id="M06"></a> M-06 | Attacker can  sway price to the point of liquidation for users by manipulating  liquidity.   

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L191

#### Description:

A user can manipulate the price by adding and removing liquidity according to their own discretion. Through this action, they can potentially force other users into liquidation by creating a significant swing in the price.

Essentially, an attacker with substantial liquidity can manipulate the market, intentionally leading other users to face liquidation.



#### Recommendation:


Some perpetual protocols implement a cap on the amount of Open Interest (OI) that a single user can hold. This limitation aims to restrict the extent to which a user can manipulate a specific market.

For instance, with a 50x leverage position, an attacker would need to move the price by 0.2% to trigger liquidations. If the cap prevents a single position from influencing the OI by 0.2% or more, it would significantly hinder a user's ability to force others into liquidation.

By implementing such a cap, the protocol can reduce the impact of manipulation and enhance the stability and fairness of the market.


#### Resolution:

### <a id="M07"></a> M-07 | Users can close position when they should be liquidated to avoid liquidation fees.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L238

#### Description:

When a user gets liquidated, they are required to pay a liquidation fee, which is part of how liquidity providers (LPs) get compensated.

To avoid paying the liquidation fee, a user can set a limit close order at the price they would have been liquidated. By doing this, they can close their position instead of going through the liquidation process. However, the user would still lose the collateral that would have been lost in the liquidation event, but they would not pay the associated liquidation fee. As a result, this action withholds yield from the LPs who are entitled to it.

#### Recommendation:

Add a check in `closePosition()` that sees if the position should be liquidated instead of closed. 




#### Resolution:

### <a id="M08"></a> M-08 | Bad actor can grief keeper by increasing/decreasing position with dust amounts. 

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L268

#### Description:

Every time a user increases or decreases a position, the keeper is required to pay a fee to obtain a price from the Pyth network. A user can exploit this by repeatedly sending tiny amounts to create and cancel orders. The amount could be small enough that they do not incur any fees for the order.

This griefing attack would result in a loss of funds for the keeper, as they would have to pay fees for each interaction with the Pyth network initiated by the user's manipulation.


#### Recommendation:

Add a minimum increase/decrease order amount to prevent dust attacks.



#### Resolution:

### <a id="M09"></a> M-09 | Missing logic to handle confidence interval for oracles.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L114

#### Description:
When data is received from the Pyth network, it includes a confidence interval that describes the price variance between data points. A higher confidence level indicates greater reliability in the price. Conversely, a low confidence level suggests that a wide range of prices were used to calculate the reported price, making it less reliable. In such cases, there should be logic in place to revert the function if the returned confidence interval exceeds an acceptable threshold. Otherwise, the price being returned may not be accurate.

In summary, implementing a reverting mechanism based on the confidence interval helps ensure the accuracy and reliability of the price data used in the system.

For more information:
https://docs.pyth.network/documentation/pythnet-price-feeds/best-practices#confidence-intervals


#### Recommendation:

Revert if cofifence interval is not acceptable. 



#### Resolution:

### <a id="M10"></a> M-10 | Position can't close when fees are greater than collateral.   

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L188

#### Description:


Since this is a perpetual protocol where positions can be opened indefinitely, there is no cap on the amount of fees a user can accumulate. This can lead to an issue when a user, who is in profit, tries to close a position with fees that exceed their collateral.

In such a scenario, the close attempt will fail. Consequently, even if a user is in profit, they will be unable to access those profits because fees are deducted solely from the collateral. This limitation restricts the user's ability to withdraw their profits, hindering their overall experience within the protocol.

#### Recommendation:

Allow users in profit to use part of their profits to pay the the closing fees. 



#### Resolution:

### <a id="M11"></a> M-11 | Users will be charged fees when market is closed.  

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L124

#### Description:

In certain instances, markets within the protocol may track assets such as gold or stocks. During these times, the market will often be closed, such as on weekends or outside of operating hours. During this period, users will not be able to take any action with their position, and the value of the asset will remain unchanged. However, users will still be charged fees during this time.

These fees can become significant if the position is highly leveraged and the market is closed for consecutive days, leading to a loss of funds for the users during this time.


#### Recommendation:

Do not charge fees when the market is closed. Some protocls change the fee rate to 0 during these times. That could potentially be an option here. 

#### Resolution:


### <a id="M12"></a> M-12 | Keepers can liquidate when protocol is paused. 

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L555

#### Description:


When the protocol is paused, the prices may still change despite the pause. As a result, the value of users' positions can be altered during the pause period. Unfortunately, users are unable to respond to these price changes since all actions are halted during the pause.

This situation creates a potential risk where users' positions can transition to a liquidatable state while the protocol is paused. Consequently, keepers can still initiate liquidations during this time, causing users to lose funds for circumstances entirely beyond their control.


#### Recommendation:

Add the `whenNotPaused` modifier to `liquidatePosition()`.

### <a id="M13"></a> M-13 | Outdated `Pyth` prices used in `PariFinace`.
#### Lines of Reference.

- [Click here](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L108C1-L108C105)
#### Description.
It is stated in the `Pyth` Docs [`Link Here`](https://docs.neonfoundation.io/docs/developing/integrate/oracles/integrating_pyth) that: 
> To use Pyth prices, you must call the function updatePriceFeeds, which submits the price update data to the Pyth contract in your target chain. ----> Your contract interacts with the Pyth contract to request a data refresh. This interaction also provides an opportunity to validate that the updates you received are authentic.

All this stated above must be done(The Pyth contract should be updated before it is queried) the [`updatePythPrice()`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L437C14-L437C29) should be called before querying the Pyth Contract that holds this updated data(price). And the developers are well aware of this, as they did so in the [`settleOrder`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/OrderManager.sol#L547C1-L547C52) & [`liquidatePosition`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/OrderManager.sol#L556C3-L556C52) functions respectively, however, Some inconsistencies are present in critical functions as there are instances where the Pyth was queried without updating it at all, and this will lead to the use of outdated prices in certain Functions, and calculations that would affect the protocol.


**Instances Fully Described**


- This issue can be seen in the `PariFiForwarder.sol`
 [`_chargeGasFees`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L108C1-L108C105) where it converts from one currency to another via the [`priceFeed::convertTokenToToken`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L383C1-L393C6) using `Pyth` data(price) without updating it first.
 ```solidity
            if (nativeToken != erc20Token) {
                gasCostInToken = priceFeed.convertTokenToToken(nativeToken, gasCostInToken, erc20Token);
            }
```
You can see that the pyth isn't updated when used for conversion.
```solidity
   function convertTokenToToken(address tokenA, uint256 amountA, address tokenB)
        external
        view
        returns (uint256 amountB)
    {
        uint256 tokenAPrice = getLatestPriceToken(tokenA);
        uint256 tokenBPrice = getLatestPriceToken(tokenB);

```

here it uses outdated prices for token conversion due to the pyth price isn't updated.

#### Reccomendation.
Update the `Pyth` contract first before querying for the price. 
Use the [`updatePythPrice()`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L437C14-L437C29) in all the instances I gave above to ensure the Latest and up-to-date price gotten from the `Pyth contract`.

### <a id="M14"></a> M-14 | Users who sign transactions are charged more `gas fees` than supposed to.
#### Lines of Reference.
- `Line 1` ~ [Click here](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L170C6-L170C37)
- `Line 2` ~ [Click here](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L200C1-L200C68)
- `Line 3` ~ [Click here](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L103C1-L112C95)
#### Description.
After the execution of a transaction by a relayer, via the `PariFiForwarder::execute`. Users are charged for gas fees via the `_chargeGasFees()` function, however and incorrect value of the `gasCostInToken` is charged from the users.
This is due to an incorrect amount of `gasSent` is used to calculate the amount of `gasCostInToken` to be charged from the user.
This vulnerability exists in `PariFiForwarder.sol`, in the `execute()` function. 
```solidity
        // @audit the gasSent is set as the gasleft() however, that is not the amount of gas actually fowarded via the external call made below.
          uint256 gasSent = gasleft();
         // ----- some code --------

        (bool success, bytes memory returndata) = transaction.toAddress.call{
            gas: transaction.minGas,
            value: transaction.txValue
        }(abi.encodePacked(transaction.txData, transaction.fromAddress));
        // --------- some checks are made --------------
       // @audit here the gasSent is used to charge users, via the _chargeGasFees() function.
        _chargeGasFees(feeToken, transaction.fromAddress, gasSent);
        return (success, returndata);

```
In the `_chargeGasFees()` function, you can see that it uses the `gasSent` to calculate the `gasCostInToken` that will be charged from a user.
```solidity
    function _chargeGasFees(address erc20Token, address payer, uint256 gasSent) internal {
        if (!approvedTokens[erc20Token]) revert LibError.InvalidToken();

        if (gasPremium > 0 && address(priceFeed) != address(0) && metaTxFeeReceiver != address(0)) {
            uint256 gasCostInToken = ((gasSent - gasleft()) * tx.gasprice * gasPremium) / MAX_FEE;

            // Only calculate when erc20Token is not native token
            if (nativeToken != erc20Token) {
                gasCostInToken = priceFeed.convertTokenToToken(nativeToken, gasCostInToken, erc20Token);
            }

            if (gasCostInToken > 0) {
                IERC20(erc20Token).safeTransferFrom(payer, metaTxFeeReceiver, gasCostInToken);
                emit MetaTxFeeCharged(payer, erc20Token, gasCostInToken);
            }
        }
    }
```

- `1` ---> The `gasSent` is set to the `gasleft()`,cached at the beginning of the function call in `execute()`.
- `2` ---> The `gasSent` is is passed into the `_chargeGasFees()` function after the `external call` is made.
- `3` ---> The `gasSent` is used to calculate the `gasCostInToken` which is used to charge users for the `gas fees` used in the `external call` made by the `execute()` function.
- `4` ---> The `gasCostInToken` is charged from the user who signed the transaction.

The issue lies in `2`, where the `gasSent` passed into the `_chargeGasFees()` is assumed to be the actual amount of gas forwarded to be used by the `external call` via the `execute()` function.

**NOTE** ---> The `gas fee` to be charged from users in a meta-transaction is the `gas` forwarded to the external call, However `transaction.minGas` only gives the limit but the actual amount sent to the `call` is actually lower. 
As The `EIP-150` defines the "all but one 64th rule" which states that always at least `1/64` of the gas is still not used when the transaction is sent via a `call`. Therefore, in `3` the `gasSent` is overstated by `1/64`, thereby making the `gasCostInToken` that is calculated with the `gasSent`(which is used to represent the gas forwarded and used by the call) above the `gas` that was actually used in the `call` by the user, Therefore `4` should not be so as users are charged an incorrect amount from what is actually used in the `external call` via the `execute()` function.

The `Impact` here is that users are charged more `gas fees` than what is actually used in the `external call` via the `execute()` function thereby overpaying for fees.

#### Reccomendation.
The recommendation is straightforward, it would be to calculate the `gasCostInToken` in the `_chargeGasFees()`  like this.
```solidity
    function _chargeGasFees(address erc20Token, address payer, uint256 gasSent) internal {
        if (!approvedTokens[erc20Token]) revert LibError.InvalidToken();

        if (gasPremium > 0 && address(priceFeed) != address(0) && metaTxFeeReceiver != address(0)) {
        uint256 gasCostInToken = (((gasSent * 63/64) - gasleft()) * tx.gasprice * gasPremium) / MAX_FEE;
```
Calculating the `gasCostInToken` with `63/64` of the `gasSent` going according to the `1/64` Gas rule. 




### <a id="M15"></a> M-15 | Missing `chainID` in the signature allows for `cross-chain` replay attacks. .
#### Lines of Reference.
- https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L141C1-L153C30
- https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PariFiForwarder.sol#L22C2-L29C6
#### Description.
Since a transaction is not signed nor verified using the `chain_id`, a valid signature that was used on one `chain` could be copied by an `attacker/Malicious Relayer` and propagated onto another `chain`, where it would also be valid for the same `user` & `contract address`! To prevent `cross-chain` signature replay attacks, smart contracts must validate the signature using the `chain_id`, and users must include the `chain_id` in the message to be signed. In `PariFiForwarder.sol` transaction struct the `chainId` is missing which means that the same Transaction can be replayed on a different `chain` for the same smart contract.

You can see here, that the Transaction struct doesn't contain a parameter for the `chain_ID`, meaning transactions are signed without a `chain_ID`.
```solidty
    struct Transaction {
        address fromAddress;
        address toAddress;
        uint256 txValue;
        uint256 minGas;
        uint256 userNonce;
        bytes txData;
    }
```
And in the `verify()` it also doesn't check if the transaction is not from the chain ist's deployed on.
```solidty
    function verify(Transaction calldata transaction, bytes calldata signature) public view returns (bool) {
        if (_nonces[transaction.fromAddress] != transaction.userNonce) revert LibError.InvalidNonce();

        address signer = _hashTypedDataV4(
            keccak256(
                abi.encode(
                    _TYPEHASH,
                    transaction.fromAddress,
                    transaction.toAddress,
                    transaction.txValue,
                    transaction.minGas,
                    transaction.userNonce,
                    keccak256(transaction.txData)
                )
            )
        ).recover(signature);
```
Malicious relayers can simply replay traders' orders on a different chain where it could be profitable for them, should be a High severity issue but i'll leave it to the judges.    
#### Reccomendation.
As specified by the [`EIP4337`](https://eips.ethereum.org/EIPS/eip-4337) standard to prevent replay attacks ... the signature should depend on `chain_ID`

### <a id="M16"></a> M-16 | Centralization Risk (Add anything else you want here, try and keep it consise)

Below you will find a list of the centralization risk associated with this protocol: 

- Admin has pause / unpause functoinality.
- Keepers will initially be controlled by the protocol.
- Updating role can lead to critical roles being lost 
- Fees amounts can change at protocols discresion (Within immutable bounds)
- Fee reciever address can be changed by protocol


Although there are centralization risk associated with this protocol. Parifi has done a good job keeping the admin privilages within reasonable bounds and has for the most part limited what an admin can do to harm the protocol. 

Reccomendation: 
- In regards to updating the multisig, Use a boolean value to track if a new multisig has been proposed, and check if it has been proposed at the beginning of the function call while ensuring against proposedMultisig being the zero address.

- Where applicable make address changes a 2-step process. Where the first step the admin sets a "proposedAddress". And the second step the "proposedAddress" can approve the given role. In general 2-step changes are seen as best practice and more secure. 

### <a id="L01"></a> L-01| `Eth` will get stuck in `Pricefeed.sol`.
#### Lines of Reference.
- https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L519C1-L519C34
#### Description.
In `PariFinance` the `PriceFeed.sol` contract can accept `ETH` due to the presence of the `recieve()`, but without a means of withdrawing it any `ETH` sent to it intentionally or Otherwise will be forever lost.
```solidity
    receive() external payable {}
```
with the presence of this function, it shows that the developers intend for this contract to be able to receive `ETH`, however, there is no means of pulling any `ETH` in the contract out.
#### Reccomendation.
Add a function to withdraw any `ETH` balance to the `ADMIN` or anyone in charge.
```solidity
    function withdrawETH() external onlyAdmin {
        address payable sender = payable(msg.sender);
        uint256 contractBalance = address(this).balance;
        
        require(contractBalance > 0, "No ETH to send");
        sender.transfer(contractBalance);
     
    }
```
And if the developers didn't intend that the contract to receive `ETH`, the `receive()` Function should be removed completely, as to avoid `ETH` from getting stuck in it.

#### Resolution:

-----------------

____

## Disclaimer
> 
> This report is not, nor should be considered, an “endorsement” or “disapproval” of any particular project or team. This report is not, nor should be considered, an indication of the economics or value of any “product” or “asset” created by any team or project that contracts Guardian Defender to perform a security assessment. This report does not provide any warranty or guarantee regarding the absolute bug-free nature of the technology analyzed, nor do they provide any indication of the technologies proprietors, business, business model or legal compliance.
> 
> This report should not be used in any way to make decisions around investment or involvement with any particular project. This report in no way provides investment advice, nor should be leveraged as investment advice of any sort. This report represents an extensive assessing process intending to help our customers increase the quality of their code while reducing the high level of risk presented by cryptographic tokens and blockchain technology.
> 
> Blockchain technology and cryptographic assets present a high level of ongoing risk. Guardian Defender’s position is that each company and individual are responsible for their own due diligence and continuous security. Guardian Defender’s goal is to help reduce the attack vectors and the high level of variance associated with utilizing new and consistently changing technologies, and in no way claims any guarantee of security or functionality of the technology we agree to analyze.
> 
> The assessment services provided by Guardian Defender is subject to dependencies and under continuing
> development. You agree that your access and/or use, including but not limited to any services, reports, and materials, will be at your sole risk on an as-is, where-is, and as-available basis. Cryptographic tokens are emergent technologies and carry with them high levels of technical risk and uncertainty. The assessment reports could include false positives, false negatives, and other unpredictable results. The services may access, and depend upon, multiple layers of third-parties.
> 
> Notice that smart contracts deployed on the blockchain are not resistant from internal/external exploit. Notice that active smart contract owner privileges constitute an elevated impact to any smart contract’s safety and security. Therefore, Guardian Defender does not guarantee the explicit security of the audited smart contract, regardless of the verdict.

<br/>

____

## About

Guardian Defender is a competitive platform for teams of practicing auditors guided by Guardian Audits.

To learn more, visit https://lab.guardianaudits.com

To view the Guardian Defender audit portfolio, visit https://github.com/GuardianAudits/DefenderAudits![]
