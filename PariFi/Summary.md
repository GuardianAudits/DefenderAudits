![](https://hackmd.io/_uploads/Hyv4GdsYn.jpg)


__Audit Firm__: Guardian Defender

__Client Firm__: PariFi

__Prepared By:__ Owen Thurm

__Delivery Date:__ August 31st, 2023

<br />

PariFi engaged Guardian Defender to review the security of its Smart Contract system. From June 28th to July 20th, a team of 3 auditors reviewed the source code in scope. All findings have been recorded in the following report.

__PariFi__ engaged Guardian Defender to review the security of its Smart Contract system. From __the 28th of June__ to __the 20th of July__, __3__ teams of __8__ total auditors reviewed the source code in scope. All Critical/High/Medium findings have been recorded in the following report, for Low/Informational/Gas findings refer to the individual team reports in this directory.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | PariFi                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | https://github.com/GuardianAudits/PariFiDefenderAudit/tree/main                               |
| Commit       | [835c98b](https://github.com/GuardianAudits/PariFiDefenderAudit/commit/835c98b66fecfe072750c400ba3ac1b0f6a07ebd) |


| Delivery Date     | August 31st, 2023       |
|-------------------|--------------------------------|
| Audit Methodology |Manual Review Static Analysis |


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 11     |   0     | 0        | 0            | 0                  | 11        |
| [High](#High)        | 5     | 0       | 0        | 0            | 0                  | 5        |
| [Medium](#Medium)    | 15     | 0       | 0        | 0            | 0                  | 15        |

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
| [C-01](#C01)  | Malicious user can take advantage of the system and gain a large profit                                      | Logical Error                 | Critical     | Resolved |
| [C-02](#C02)  | Native tokens will be stuck in the PariFi forwarder if the transaction execution was not successful.                                     | Logical Error                 | Critical     | Resolved |
| [C-03](#C03)  | Users can be gas-griefed by relayers by setting an extremely high tx.gasprice.                                    | Logical Error                 | Critical     | Resolved |
| [C-04](#C04)  | The borrow rate calculation formula is flawed.                                     | Logical Error                 | Critical     | Resolved |
| [C-05](#C05)  | The price deviation formula is flawed.                                  | Logical Error                 | Critical     | Resolved |
| [C-06](#C06)  | Users will be liquidated even if they should not be.                                     | Logical Error                 | Critical     | Resolved |
| [C-07](#C07)  | Accrued borrowing fees are incorrectly calculated.                                     | Logical Error                 | Critical     | Resolved |
| [C-08](#C08)  | Liquidity providers can sandwich traders when taking profits to avoid losses.                                     | Logical Error                 | Critical     | Resolved |
| [C-09](#C09)  | Fees should be updated before any other accounting is done.                                                        | Logical Error         | Critical     | Resolved |
| [C-10](#C10)  | Losses get stuck in contract when position is closed.                                                        | Logical Error         | Critical     | Resolved |
| [C-11](#C11)  | Users can avoid losing collateral when at a loss by decreasing position.                                       | Logical Error         | Critical     | Resolved |
| [H-01](#H01)  | An adversary can DoS orders creation in the order manager.        | Logical Error     | High | Resolved
| [H-02](#H02)  | The order creation deadline is incorrectly checked. | Logical Error         | High     | Resolved |
| [H-03](#H03)  | Cumulative fees should be updated before configuration values are changed by the admin. |  Logical Error       | High     | Resolved |
| [H-04](#H04)  | Profitable positions that become undercollateralized after a fee is applied cannot be liquidated. | Logical Error   | High   | Resolved |
| [H-05](#H05)  | Transaction EIP712 object is missing a deadline property.                                                     | Missing Deadline      | High   | Resolved |
| [M-01](#M01)  | updatedAvgPrice doesnt allow users full control of decreasing their position                                                     | Logical Error         | Medium   | Resolved |
| [M-02](#M02)  | Users can decrease the entirety of their collateral leaving them unable to get their profits                                                     | Logical Error         | Medium   | Resolved |
| [M-03](#M03)  | Fees keep accumulating even if the protocol is paused.                                                     | Logical Error         | Medium   | Resolved |
| [M-04](#M04)  | Stock splits are not accounted for | Logical Error         | Medium   | Resolved |
| [M-05](#M05)  | Users can be gas-griefed by admin by setting an extremely high gasPremium.                                                     | Logical Error         | Medium   | Resolved |
| [M-06](#M06)  | Relayers can be gas-griefed.                                                     | Logical Error         | Medium   | Resolved |
| [M-07](#M07)  | Missing check for minimal collateral when the opening fee is reduced from provided collateral on order creation.                                                  | Logical Error         | Medium   | Resolved |
| [M-08](#M08)  | Missing check for minimal collateral when decreasing a position.                                                  | Logical Error         | Medium   | Resolved |
| [M-09](#M09)  | The protocol admin can break core functionalities resulting in the loss of funds.                                                      | Logical Error         | Medium   | Resolved |
| [M-10](#M10)  | Outdated Pyth prices used in PariFinace.                                                   | Outdated Prices        | Medium   | Resolved |
| [M-11](#M11)  | Keepers can liquidate when protocol is paused.                                                  | Centralization         | Medium   | Resolved |
| [M-12](#M12)  | Users will be charged fees when market is closed.                                                   | Fees         | Medium   | Resolved |
[M-13](#M13)  | Attacker can sway price to the point of liquidation for users by manipulating liquidity.                                                   | Protocol Manipulation        | Medium   | Resolved |
[M-14](#M14)  | Order can fail becasue fees are deducted between validation checks.                                                  | Logical Error         | Medium   | Resolved |
| [M-15](#M15)  | Gas compensation for relayer does not consider the initial transaction gas cost.                                                  | Gas Remuneration         | Medium   | Resolved |



## <a id="Critical"></a>Critical


### <a id="C01"></a> C-01 Malicious user can take advantage of the system and gain a large profit

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L140-L149

###  Description:
`diffBps > availableMarkets[_marketId].allowedPriceDeviation` is implemented incorrectly
In a case where `diffBps > allowedPriceDeviation` we give users more favorable prices instead of less favorable prices.

This goes against the core principles of the system when this scenario happens it allows users to create an order that will be in an instant profit.

```
PoC

function test_Wrongs_Price() public {
        uint256 collateralAmount = 10 ether;
        uint256 currentMarketPrice = 3000;
        uint256 currentMarketPrice2 = 3040;
        bool isLong = true;
        uint256 initialBalance = 10 ether;

        uint256 marketMaxLeverage = orderManager.getMarket(ETH_MARKET_ID).maxLeverage;
        uint256 orderSize = collateralAmount + 1 ether;
        uint256 openingFeeAmount = orderSize * DEFAULT_OPENING_FEE / MAX_FEE;

        collateralAmount += openingFeeAmount;

        // NOTE You will have to comment out the line updatePriceChainlink(chainlinkEthFeed, price);  
        // line in function _settleorder
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);

        // Price of chainlink deviates from by 40 
        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice2);

        deal(WETH, TRADER_1, collateralAmount);

        bytes32 orderId = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, !isLong, 0);

        OrderDS.Order memory orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.OPEN_NEW_POSITION,
            isLong: false,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: collateralAmount,
            orderSize: orderSize,
            expectedPrice: 0,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);
        ERC20(WETH).approve(address(orderManager), collateralAmount);

        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();

        // TRADER_1s Short gets filled at 3040
        _settleOrder(orderId,currentMarketPrice);

        skip(1);
        // Price deviates stops and goes back to 3000
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);
        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);

        orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.CLOSE_POSITION,
            isLong: false,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: collateralAmount,
            orderSize: orderSize,
            expectedPrice: 0,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);
        orderManager.createOrder(orderStruct, address(0));
        vm.stopPrank();

        // TRADER_1 closes the position at a profit 
        _settleOrder(orderId,currentMarketPrice);

        assertLt(initialBalance, ERC20(WETH).balanceOf(TRADER_1), "!Profit");
        assertEq(initialBalance, 10 ether, "!Incorrect Balance");
        // Check that TRADER_1 created a profit
        assertEq(ERC20(WETH).balanceOf(TRADER_1), 10135665968001393998, "Check Balance After");
    }
```

###  Recommendation:
Change in implementation to the following.

```
if (diffBps > availableMarkets[_marketId].allowedPriceDeviation) {
  // If the price is more than the allowedPriceDeviation, the trader gets the less 
favourable price
  uint256 lowerLimit = primaryPrice < secondaryPrice ? primaryPrice : secondaryPrice;
  uint256 upperLimit = primaryPrice < secondaryPrice ? secondaryPrice : primaryPrice;
  if (_isLong) {
    updatedPrice = upperLimit;
  } else {
    updatedPrice = lowerLimit ;
  }
}
```
###  Resolution:

PariFi Team: The issue was resolved with the following PRs:

https://github.com/Parifi/parifi-contracts-internal/pull/39
https://github.com/Parifi/parifi-contracts-internal/pull/41


### <a id="C02"></a> C-02 Native tokens will be stuck in the PariFi forwarder if the transaction execution was not successful.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L185

#### Description:

If the transaction in `PariFiForwarder.execute` was not successful, the function will return a status of `false` (+ the returned data) instead of reverting.

The problem is that the native tokens sent with the transaction to the `transaction.toAddress` will remain locked in the `PariFiForwarder` forever, as they are not returned to the relayer.

#### Recommendation:

Consider returning the `msg.value` back to the relayer if the transaction was not successful.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/0b0e5289a3c5e17fa70ee62befb94dcb9c1617e9



### <a id="C03"></a> C-03 Users can be gas-griefed by relayers by setting an extremely high tx.gasprice.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L104

#### Description:

The gas compensation is calculated in the following way in PariFiForwarder:

```solidity
uint256 gasCostInToken = ((gasSent - gasleft()) * tx.gasprice * gasPremium) / MAX_FEE;
```

The transaction object that a user signs, however, contains only the following properties:

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

The problem is that there's no maximum `tx.gasprice` value signed by the user, which means that relayers can specify as high a `tx.gasprice` as they want in order to incur a loss for the user. Having in mind that the `execute` function has no access control, this can be easily abused by an attacker, especially if `type(uint256).max` allowance is given to the PariFiForwarder contract by users.

#### Recommendation:

Consider adding a `maxTxGasPrice` property to the `Transaction` struct.

#### Resolution:

PariFi Team: The recommendation was implemented in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/318dcb87bd54742db2124e340613cad1585cb419



### <a id="C04"></a> C-04 The borrow rate calculation formula is flawed.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L198-L202

#### Description:

Consider the following example:

1. `baseCoeff[marketId]` = `125`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L75)
2. `utilizationBps` = `8000 * 100 = 800000`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L237)
3. `baseConst[marketId]` = `100`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L76)
4. `baseBorrowRate` = `(1e18 * 125 * 800000 * 800000 + 100) / 1e12` = `80000000000000000000` = `80e18`
5. This seems wrong as the `baseBorrowRate` exceeds `1e18`, while `1e18` is the equivalent of a 100% base borrow rate
   per year. Also, the `baseConst` of `100` is completely ineffective since when divided by `1e12`, it will round down
   to `0`.

#### Recommendation:

Consider whether the problem lies within the values used in the deployment script. If the problem is within the `getBaseBorrowRatePerSecond` function, mitigate it so that it returns an appropriate value.

Also, due to the time constraints, I cannot execute a proper test on the `getDynamicBorrowRatePerSecond`, but I assume it may be affected by a similar issue.

#### Resolution:

PariFi Team: The issue was resolved in the following pull request:

https://github.com/Parifi/parifi-contracts-internal/pull/45



### <a id="C05"></a> C-05 The price deviation formula is flawed.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L191

#### Description:

Consider the following example:

1. `deviationCoeff[marketId]` = `40000`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L72)
2. `utilizationBps` = `8000`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L237)
3. `deviationConst[marketId]` = `0`
   [(ref)](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/script/PariFiDeployment.s.sol#L73)
4. `deviationPoints` = `40000 * 8000 * 8000 + 0` = `2560000000000` = `2.560e12`
5. This is clearly wrong as the `deviationPoints` exceed `1e12`, which will cause an
   [underflow error](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L151-L156) in
   the order manager.

#### Recommendation:

Consider whether the problem is that `40000` used for `deviationCoeff` in the deployment script is causing the issue, and if this is the case, add an upper limit check in the contract. If the problem lies within the `getPriceDeviation` function, mitigate it to return an appropriate value.

#### Resolution:

PariFi Team: The issue was resolved in the following pull request:

https://github.com/Parifi/parifi-contracts-internal/pull/45



### <a id="C06"></a> C-06 Users will be liquidated even if they should not be.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L410-L420

#### Description:

The `market.liquidationThreshold` is used to determine whether a position is undercollateralized or not. This [check](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L410) is made in `_liquidatePosition` to determine whether or not the user position can be liquidated.

However, even if the check does not pass, the user position will still be liquidated because the function does not revert the transaction:

```solidity
if (lossInCollateral > liquidationThreshold) {
    IERC20(market.depositToken).safeTransfer(feeManager, lossInCollateral);
    IFeeManager(feeManager).distributeFees(market.depositToken);
}

// Transfer any remaining collateral to the user
uint256 userBalance;
if (userPosition.collateralAmount > lossInCollateral) {
    userBalance = userPosition.collateralAmount - lossInCollateral;
    IERC20(market.depositToken).safeTransfer(userPosition.userAddress, userBalance);
}

dataFabric.updateMarketData(market.marketId, userPosition.positionSize, userPosition.isLong, false);

delete openPositions[_positionId];
emit PositionLiquidated(_positionId, userBalance);
```

#### Recommendation:

Consider reverting `if (lossInCollateral <= liquidationThreshold)` instead of continuing with the logic.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/240ddcbfc0ad01ad24baf21b43d96f8b63d41579



### <a id="C07"></a> C-07 Accrued borrowing fees are incorrectly calculated.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L218
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L689-L696
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L160-L170
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L124-L141

#### Description:

Every time a user modifies their position in the OrderManager, the `userPosition.lastCumulativeFee` is updated and set to the cumulative value that is being tracked in `DataFabric.updateCumulativeFees`. Then, to calculate the difference between the `lastCumulativeFee` and the current `baseFeeCumulative` value, the `getAccruedBorrowFeesInMarket` function is used, which has the following logic:

```solidity
function getAccruedBorrowFeesInMarket(bytes32 _positionId) public view returns (uint256 accruedBorrowFees) {
    Position memory userPosition = openPositions[_positionId];
    uint256 currFeeCumulative = dataFabric.getCurrentFeeCumulative(userPosition.marketId, userPosition.isLong);
    uint256 accruedFeesCumulative = _getDiff(currFeeCumulative, userPosition.lastCumulativeFee);

    accruedBorrowFees =
        accruedFeesCumulative.mulWadDown(userPosition.positionSize * (block.timestamp - userPosition.lastTimestamp));
}
```

The `dataFabric.getCurrentFeeCumulative` itself returns the sum of the values stored for the base cumulative borrow fee and the dynamic one. They are calculated in `updateCumulativeFees` in the following way:

```solidity
baseFeeCumulative[marketId] = baseFeeCumulative[marketId] + timeDelta * getBaseBorrowRatePerSecond(marketId);

if (totalLongs[marketId] > totalShorts[marketId]) {
    dynamicLongFeeCumulative[marketId] =
        dynamicLongFeeCumulative[marketId] + timeDelta * getDynamicBorrowRatePerSecond(marketId);
} else {
    dynamicShortFeeCumulative[marketId] =
        dynamicShortFeeCumulative[marketId] + timeDelta * getDynamicBorrowRatePerSecond(marketId);
}
```

The `baseFeeCumulative`, `dynamicLongFeeCumulative`, and `dynamicShortFeeCumulative` are updated using the `timeDelta`. Therefore, the `_getDiff(currFeeCumulative, userPosition.lastCumulativeFee)` represents the actual change for the time period between the last (for the user position) and current update.

However, `accruedBorrowFees` is calculated by multiplying the `accruedFeesCumulative` by the position size (and dividing by the appropriate precision). The code then multiplies this value once more by the time difference, which will return a completely wrong result because the value of `accruedFeesCumulative` already considers the time passed (it's not per second).

#### Recommendation:

Consider removing the ` * (block.timestamp - userPosition.lastTimestamp)` from the calculation of `accruedBorrowFees`.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/c8d88b9bfae4fa9068da6d27e2a594822aa466ed


### <a id="C08"></a> C-08 Liquidity providers can sandwich traders when taking profits to avoid losses.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L178-L183
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L249
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L81-L102

#### Description:

When a trader wants to close their position on profit, the profit is taken from the liquidity providers who have staked deposit/collateral tokens in the corresponding MarketVault through `withdrawUserProfits`. This will automatically decrease the value of liquidity providers' shares, since the MarketVault is a standard ERC4626 vault that uses its balance of the underlying `asset` to determine the shares' value.

The problem is that a large liquidity provider can build a script to monitor the mempool and withdraw their shares every time when the keeper calls `settleOrder` for closing a position that is on profit. That way, the liquidity provider can avoid almost any losses.

Similarly, a large liquidity provider can arbitrage by depositing right before borrowing fees are distributed and withdraw after that, effectively stealing from long-term liquidity providers' earnings.

#### Recommendation:

Consider allowing liquidity providers to withdraw their shares only after a certain lock period from their deposit timestamp.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/d858f8f4b46fbdf8a99dea020ac40eee49852f2f


### <a id="C09"></a> C-09 Fees should be updated before any other accounting is done.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L249-L271
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L125

#### Description:

Currently, the borrowing fees are updated at the end of every operation that changes the `totalLongs` or `totalShorts` through `DataFabric.updateMarketData`.

The problem is that `updateCumulativeFees` accounts for the fees that were accrued in the time between this call and the previous update:

[src/DataFabric.sol#L125](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L125)

```solidity
uint256 timeDelta = block.timestamp - feeLastUpdatedTimestamp;
```

However, `updateCumulativeFees` is called at the end of `updateMarketData`, after the `totalLongs` or `totalShorts` were updated. Therefore, the calculated value in `getBaseBorrowRatePerSecond` and `getDynamicBorrowRatePerSecond` will not be correct for the `timeDelta` as they depend on the values of `totalLongs` or `totalShorts` BUT for the passed time (`timeDelta`), which should not consider the update made in `updateMarketData`.

#### Recommendation:

Consider calling `updateCumulativeFees` at the beginning of `updateMarketData` instead of the end.

#### Resolution:

PariFi Team: The issue was resolved in the following commits:

https://github.com/Parifi/parifi-contracts-internal/commit/58a820ae22c0e7dce1bfd426c10818e961b9b483
https://github.com/Parifi/parifi-contracts-internal/commit/1d22cfe86406573126fde3310869a23c834754f7



### <a id="C10"></a> C-10 | Losses get stuck in contract when position is closed.  

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L252-L257

#### Description:

When a user closes a position at a loss, the loss will correctly be deducted from the user's position before transferring back any collateral. However, the loss does not get transferred to the Fee Manager and ends up staying in the contract unaccounted for. As a result, it remains locked in the contract permanently. This is an issue because those funds are owed to the liquidity providers.



#### Recommendation:

After the loss has been deducted from the users collateral, transfer that amount to the fee manager. 



#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/38f0c2349380abb3a405244d299a2e21037e47f6



### <a id="C11"></a> C-11 | Users can avoid losing collateral when at a loss by decreasing position.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L324-L331

#### Description:

When users are at a loss and need to close a position, the loss is deducted from their position, and the remaining collateral is returned to the user. However, users have found a way to avoid losing their collateral by decreasing their position before closing it. Currently, there is no check to see if a user is at a loss when decreasing their position. The only requirement is that the position's leverage is valid after the decrease.

As a result, users can decrease their position to a point where there is no loss, and the amount they decreased the position by will be returned to them. Then, when they close the position, the remaining collateral will also be sent back to them. This allows them to bypass any loss to their collateral that they should have incurred.

This loophole allows users to protect their collateral by strategically decreasing their position before closing it, effectively avoiding any losses they would have otherwise experienced.



#### Recommendation:

Deduct losses from positon before decreasing position. 


____

## <a id="High"></a>High


### <a id="H01"></a> H-01 An adversary can DoS orders creation in the order manager.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderDS.sol#L31
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L521-L523

#### Description:

In OrderDS.sol, it is mentioned how an order id should be calculated:

```solidity
bytes32 orderId; // keccak256 hash of marketId, marketAddress, userAddress, positionDirection, and sequence
```

However, nowhere in the actual OrderManager contract, this is computed nor verified. Instead, it is simply passed by the order creator.

This opens up an attack vector where an adversary can simply front-run the creation of orders that they don't want to be created by passing the same `orderId` that the victim is about to use. Then, the order can either be closed by the attacker releasing almost no loss or just be kept in case the attacker makes use of it.

This can repeat as many times as the attacker wants with any order that is about to get created effectively causing a DoS.

#### Recommendation:

Consider computing the `orderId` in the OrderManager contract as is done with the `positionId`s.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/3ca9c3b379eea9d73c175d636d02830607b58a20



### <a id="H02"></a> H-02 The order creation deadline is incorrectly checked.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L526

#### Description:

The order deadline is meant to prevent orders from being executed after a certain timestamp that the creator picks.

However, the deadline check is currently performed in the `createOrder` instead of `_settleOrder`, which makes the deadline completely useless and risks users' funds.

#### Recommendation:

Consider moving the check on line 526 to the `_settleOrder` function.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/573a2457c0b71a0fd91134be7558611455fc8461



### <a id="H03"></a> H-03 Cumulative fees should be updated before configuration values are changed by the admin.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L281-L343

#### Description:

There are multiple configuration variables like `dynamicCoeff`, `baseCoeff`, `deviationCoeff`, etc. that can be updated by the protocol admin. All these variables are used to determine the borrowing fees calculated regularly in `updateCumulativeFees`.

The problem is that `updateCumulativeFees` is not called before changing the configuration values, which means that the new values would apply even for the time between the `feeLastUpdatedTimestamp` and the current `block.timestamp`, which is not desired and will result in wrong accounting.

#### Recommendation:

Consider calling `updateCumulativeFees` at the beginning of all admin setter functions in DataFabric.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/58a820ae22c0e7dce1bfd426c10818e961b9b483



### <a id="H04"></a> H-04 Profitable positions that become undercollateralized after a fee is applied cannot be liquidated.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L395

#### Description:

Currently, you cannot liquidate a position that is on profit.

However, it's possible that a position has a slight profit but a large amount of liquidation, closing, and borrowing fees to pay, which could make it undercollateralized when the fees are deducted from the balance and profit.

Such a position will not be able to get liquidated though, which breaks an important protocol invariant.

#### Recommendation:

Consider removing the `isProfit` check in `_liquidatePosition` and then deducting the fees from the collateral, and just after that check for the liquidation threshold. Also, considering this case was not handled in the smart contract, I assume it was also not handled in the code for the keeper script, so it may be necessary to implement the needed logic there as well.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/240ddcbfc0ad01ad24baf21b43d96f8b63d41579



### <a id="H05"></a> H-05 Transaction EIP712 object is missing a deadline property.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L22-L29

#### Description:

Deadlines are usually used for any kind of user signatures to validate that the user still intends to execute the transaction at the time of actual execution.

Such a property is missing in the `Transaction` struct in `PariFiForwarder`, so if a user's transaction initially failed or was in the mempool for too long or simply executed at an unfavorable time for them in the future, it could lead to unexpected behavior and potentially loss of funds.

#### Recommendation:

Consider adding a `deadline` property to the `Transaction` struct.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/d36267f9db7eabecaae195f06c5e3aabc35cb7cb



____

## <a id="Medium"></a>Medium


### <a id="M01"></a> M-01 updatedAvgPrice doesnt allow users full control of decreasing their position

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L320-L322

#### Description:
When users are in profit the implementation of updatedAvgPrice doesn't allow users complete control of decreasing their position.

`uint256 updatedAvgPrice = (userPosition.positionSize * userPosition.avgPrice - userOrder.orderSize * _executionPrice)
 /  (userPosition.positionSize - userOrder.orderSize);`

Consider the following

TRADER_1 creates a position for 1 eth @3000
Price goes from 3000 to 6000

TRADER_1 tries to decrease for 0.6 eth

This will underflow upon settling the order.
uint256 updatedAvgPrice = (userPosition.positionSize `(1 eth)` * userPosition.avgPrice `(3000)` -
 userOrder.orderSize `(0.6 eth)` * _executionPrice `(6000)`);

- POC:
```
function test_underflow_upon_decrease() public {
        uint256 collateralAmount = 1 ether;
        uint256 currentMarketPrice = 3000;
        bool isLong = true;

        uint256 openingFeeAmount = collateralAmount * DEFAULT_OPENING_FEE / MAX_FEE;

        collateralAmount += openingFeeAmount;
        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);
        skip(1);

        deal(WETH, TRADER_1, collateralAmount);

        assertEq(ERC20(WETH).balanceOf(TRADER_1), collateralAmount);
        assertEq(collateralAmount, 1001000000000000000);

        bytes32 orderId = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong, 0);

        OrderDS.Order memory orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.OPEN_NEW_POSITION,
            isLong: isLong,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: collateralAmount,
            orderSize: collateralAmount,
            expectedPrice: 0,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);
        ERC20(WETH).approve(address(orderManager), collateralAmount);

        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();

        assertTrue(orderManager.isResolvedOrder(orderId), "order");


        _settleOrder(orderId, currentMarketPrice);

        bytes32 positionId = orderManager.getPositionId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong);

        assertTrue(orderManager.isValidPosition(positionId), "!position");

        currentMarketPrice = 6000;
        skip(1);

        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);

        orderId = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong, 0);

        orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.DECREASE_POSITION,
            isLong: isLong,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: 600000000000000000,
            orderSize: 600000000000000000,
            expectedPrice: currentMarketPrice * PRICE_FACTOR,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);

        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();
        
        // This will underflow when calculating updatedAvgPrice
/*
        uint256 updatedAvgPrice = (
            userPosition.positionSize * userPosition.avgPrice - userOrder.orderSize * _executionPrice
        ) / (userPosition.positionSize - userOrder.orderSize);

       userPosition.positionSize * userPosition.avgPrice = 300300000000000000000000000000
       userOrder.orderSize * _executionPrice = 360000000000000000000000000000
*/

        //_settleOrder(orderId, currentMarketPrice);
    } 
```




#### Recommendation:
Reformulate the calculation of `updatedAvgPrice` in a way that it can never result in an underflow.


#### Resolution:

PariFi Team: The issue was resolved in the following PR:

https://github.com/Parifi/parifi-contracts-internal/pull/47



### <a id="M02"></a> M-02 Users can decrease the entirety of their collateral leaving them unable to get their profits
#### Description:
The system allows users to decrease the entirety of their collateral leaving them unable to close their position since they won't be able to pay the fees when they have 0 collateral.

This can leave users unable to retrieve their profits.

```
PoC

function test_users_unable_to_claim_profits() public {
        uint256 collateralAmount = 1 ether;
        uint256 currentMarketPrice = 3000;
        bool isLong = true;

        uint256 openingFeeAmount = collateralAmount * DEFAULT_OPENING_FEE / MAX_FEE;

        collateralAmount += openingFeeAmount;
        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);
        skip(1);

        deal(WETH, TRADER_1, collateralAmount);

        // Check initial balance of TRADER_1
        assertEq(ERC20(WETH).balanceOf(TRADER_1), collateralAmount);
        assertEq(collateralAmount, 1001000000000000000);

        bytes32 orderId = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong, 0);

        OrderDS.Order memory orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.OPEN_NEW_POSITION,
            isLong: isLong,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: collateralAmount,
            orderSize: collateralAmount,
            expectedPrice: 0,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);
        ERC20(WETH).approve(address(orderManager), collateralAmount);

        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();

        assertTrue(orderManager.isResolvedOrder(orderId), "order");


        _settleOrder(orderId, currentMarketPrice);


        currentMarketPrice = 6000;
        skip(1);

        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);

        orderId = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong, 0);

        bytes32 positionId = orderManager.getPositionId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, true);
        OrderDS.Position memory userPosition = orderManager.getPosition(positionId);

        assertEq(userPosition.collateralAmount, 999999000000000000);

        assertEq(userPosition.positionSize, 1001000000000000000);
        
        orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.DECREASE_POSITION,
            isLong: isLong,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: 999999000000000000,
            orderSize: 0,
            expectedPrice: currentMarketPrice * PRICE_FACTOR,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);

        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();

        _settleOrder(orderId, currentMarketPrice);

        positionId = orderManager.getPositionId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, true);
        userPosition = orderManager.getPosition(positionId);

        assertEq(userPosition.collateralAmount, 0);

        assertEq(userPosition.positionSize, 1001000000000000000);

        orderId = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong, 0);
    
        orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.CLOSE_POSITION,
            isLong: isLong,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: 0,
            orderSize: 1001000000000000000,
            expectedPrice: 0,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);

        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();
        
        // Users can decrease the entirety of their collateral leaving them
        // unable to close their position since
        // feeInCollateral > userPosition.collateralAmount.
        // Leaving users unable to retrive the profits they were in.
        
        //_settleOrder(orderId, currentMarketPrice);

        assertEq(ERC20(WETH).balanceOf(TRADER_1), 999999000000000000);
    }
```

#### Recommendation:
Don't allow users to decrease the entirety of their collateral.

#### Resolution:

PariFi Team: The issue was resolved in the following PR:

https://github.com/Parifi/parifi-contracts-internal/pull/48



### <a id="M03"></a> M-03 Fees keep accumulating even if the protocol is paused

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L124-L141

#### Description:
The cumulative fees are calculated by the `updateCumulativeFees` function in `DataFabric` contract:

```solidity
function updateCumulativeFees(bytes32 marketId) internal {
  uint256 timeDelta = block.timestamp - feeLastUpdatedTimestamp;
  baseFeeCumulative[marketId] = baseFeeCumulative[marketId] + timeDelta * getBaseBorrowRatePerSecond(marketId);
  
  if (totalLongs[marketId] > totalShorts[marketId]) {
      dynamicLongFeeCumulative[marketId] =
          dynamicLongFeeCumulative[marketId] + timeDelta * getDynamicBorrowRatePerSecond(marketId);
  } else {
      dynamicShortFeeCumulative[marketId] =
          dynamicShortFeeCumulative[marketId] + timeDelta * getDynamicBorrowRatePerSecond(marketId);
  }
  
  feeLastUpdatedTimestamp = block.timestamp;
  
  emit CumulativeFeeUpdated(
      baseFeeCumulative[marketId], dynamicLongFeeCumulative[marketId], dynamicShortFeeCumulative[marketId]
  );
}
```

As you see `timeDelta` is calculated as `block.timestamp - feeLastUpdatedTimestamp` and `feeLastUpdatedTimestamp` is set to `block.timestamp` in the same tx. This means that it is also considering the blocks when the protocol was paused.

This fees are charged by `getAccruedBorrowFeesInMarket` called in `OrderManager` when: increase, decrease, liquidate and closePosition.
If a position can not be closed due to OrderManager being closed the period in which the contract is paused until it is unpaused again so that the position is closed is counted for charging fees. So it is possible to charge infinite fees.

Once the contract is unpaused the user is able to execute actions again but will be unfairly charged with the accumulated fees during the paused period.

#### Recommendation:
Update the `updateCumulativeFees` function to not consider the blocks while the contract is in 'paused' state to charge fees.

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/90ae04f35975449a9faccd2cd2161ef0cdc0fab1




### <a id="M04"> M-04 Stock splits are not accounted for</a>

#### Description:
Among many things that can be supported are equities, great care should be taken in case of forward stock splits, where the price per share is reduced and the number of shares a user owns increased. 

Other scenarios include reverse stock splits, mergers, etc.

This can leave users with large losses and gains deResolved on direction.

#### Recommendation:
carefully monitor markets where such assets are present.

#### Resolution:

PariFi Team: Acknowledged, only indexes will be supported at first and updates will be introduced before stocks are supported in the future.



### <a id="M05"></a> M-05 Users can be gas-griefed by admin by setting an extremely high gasPremium.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L244-L251
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L104

#### Description:

Similar to issue C-07, users can be gas-griefed because of the `gasPremium` property that is not included in the `Transaction` struct and therefore may be changed without the user's knowledge.

#### Recommendation:

Consider adding a `maxGasPremium` property to the `Transaction` struct.

#### Resolution:

PariFi Team: Acknowledged, keepers are trusted to not set malicious a malicious gasPremium.


### <a id="M06"></a> M-06 Relayers can be gas-griefed.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L139
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L112

#### Description:

There are currently 2 ways in which relayers can be gas-griefed for transactions execution in the `PariFiForwarder`:

1. When a transaction is submitted, a user can front-run the call from the relayers so that they revoke the approval for
   the ERC20 token that should be used for compensation.
2. A user can simply execute the transaction themselves, which will then make the relayer transaction revert because the
   nonce has been incremented.

Both attack vectors will result in the relayers losing funds because of the gas spent (before the revert) that will not be compensated.

#### Recommendation:

The second case may be solved by compensating the relayer for the gas spent so far in the transaction execution, even if it's not successful. However, a fix for the first case is not trivial.

#### Resolution:

PariFi Team: The issue was addressed in the following PR:

https://github.com/Parifi/parifi-contracts-internal/pull/46



### <a id="M07"></a> M-07 Missing check for minimal collateral when the opening fee is reduced from provided collateral on order creation.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L442

#### Description:

As explained in the previous issue M-03, there's a minimum collateral amount that has to be deposited to create an order.

However, the check for it is made at the beginning of `_createOrderNewPosition`:

```solidity
function _createOrderNewPosition(Order memory _order, Market memory market, address partner) internal {
    // Collateral checks
    if (_order.collateralAmount < market.minCollateral || _order.collateralAmount == 0) {
        revert LibError.InvalidCollateralAmount();
    }
```

This is a problem as the `_order.collateralAmount` is then decreased if there's an opening fee set:

[src/OrderManager.sol#L453-L457](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L453-L457)

```solidity
uint256 fees = (market.openingFee * _order.orderSize) / MAX_FEE;
uint256 feeInCollateral =
    priceFeed.convertMarketToTokenSecondary(market.marketId, fees, market.depositToken);

_order.collateralAmount = _order.collateralAmount - feeInCollateral;
```

Therefore, a user can still create an order with too low collateral when fees are applied.

#### Recommendation:

Consider executing the `minCollateral` check after the fees are deducted.

#### Resolution:

PariFi Team: The issue was addressed in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/568c47755305cdd82ab9225dafeeb6612e8a5ea5



### <a id="M08"></a> M-08 Missing check for minimal collateral when decreasing a position.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L442

#### Description:

Each market can have a specific amount of minimal collateral that should be deposited:

```solidity
struct Market {
    ...
    uint256 minCollateral; // Min. amount of collateral to deposit
    ...
}
```

However, the check for it is only applied when an order is created. A position's collateral can fall below that level if a user simply decreases their position with the corresponding amount of collateral tokens, which makes the check in deposit ineffective.

#### Recommendation:

Consider applying the `minCollateral` when a user decreases their position. Furthermore, a user's position's collateral is decreased when fees are charged, so it might be necessary to add such check even when a position's collateral is increased in case the fees are higher than the increase.

#### Resolution:

PariFi Team: The issue was addressed in the following PR:

https://github.com/Parifi/parifi-contracts-internal/pull/48



### <a id="M09"></a> M-09 The protocol admin can break core functionalities resulting in the loss of funds.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L871-L874
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L848-L852
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L114-L130
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L211-L213

#### Description:

The protocol is currently highly centralized as it relies on the PariFi team multisig to change important configuration values, as well as it allows them to `pause` important functionalities that will prevent users from withdrawing their funds or closing their position in the right time. It is also possible that the admin sets a blacklisted USDC (or any other token used as collateral) addresses as fee receivers to also block certain core functionalities.

#### Recommendation:

Consider improving the input validation in admin setter functions. Consider removing the `whenNotPaused` modifier from withdrawal functions like `MarketVault.withdraw`, `MarketVault.redeem`, `OrderManager.cancelResolvedOrder`.

#### Resolution:

PariFi Team: Acknowledged, the admin is a trusted multisig that is assumed to not abuse it's powers.



### <a id="M10"></a> M-10 Outdated `Pyth` prices used in `PariFinace`.
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
    }
```

here it uses outdated prices for token conversion due to the pyth price isn't updated.

#### Reccomendation.
Update the `Pyth` contract first before querying for the price. 
Use the [`updatePythPrice()`](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L437C14-L437C29) in all the instances I gave above to ensure the Latest and up-to-date price gotten from the `Pyth contract`.

#### Resolution:

PariFi Team: The issue was addressed in the following PR:

https://github.com/Parifi/parifi-contracts-internal/pull/44



### <a id="M12"></a> M-12 | Keepers can liquidate when protocol is paused. 

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L555

#### Description:


When the protocol is paused, the prices may still change despite the pause. As a result, the value of users' positions can be altered during the pause period. Unfortunately, users are unable to respond to these price changes since all actions are halted during the pause.

This situation creates a potential risk where users' positions can transition to a liquidatable state while the protocol is paused. Consequently, keepers can still initiate liquidations during this time, causing users to lose funds for circumstances entirely beyond their control.


#### Recommendation:

Add the `whenNotPaused` modifier to `liquidatePosition()`.

#### Resolution:

PariFi Team: The issue was addressed in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/f129a7c36fef0c132d3add862391311cfa4aeee9



### <a id="M13"></a> M-13 | Users will be charged fees when market is closed.  

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L124

#### Description:

In certain instances, markets within the protocol may track assets such as gold or stocks. During these times, the market will often be closed, such as on weekends or outside of operating hours. During this period, users will not be able to take any action with their position, and the value of the asset will remain unchanged. However, users will still be charged fees during this time.

These fees can become significant if the position is highly leveraged and the market is closed for consecutive days, leading to a loss of funds for the users during this time.


#### Recommendation:

Do not charge fees when the market is closed. Some protocls change the fee rate to 0 during these times. That could potentially be an option here. 

#### Resolution:

PariFi Team: We decided to implement the solution of not charging borrowing fees when the markets are closed/paused. This finding is similar to “Fees keep accumulating even if protocol is paused”. As the borrowing fees are per market and we don’t have a global variable to turn on/off these fees, we need to pause/unpause all markets individually to enable/disable borrowing fees. Hence, we have the same fix for both the findings: https://github.com/Parifi/parifi-contracts-internal/commit/90ae04f35975449a9faccd2cd2161ef0cdc0fab1



### <a id="M14"></a> M-14 | Attacker can  sway price to the point of liquidation for users by manipulating  liquidity.   

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L191

#### Description:

A user can manipulate the price by adding and removing liquidity according to their own discretion. Through this action, they can potentially force other users into liquidation by creating a significant swing in the price.

Essentially, an attacker with substantial liquidity can manipulate the market, intentionally leading other users to face liquidation.

#### Recommendation:


Some perpetual protocols implement a cap on the amount of Open Interest (OI) that a single user can hold. This limitation aims to restrict the extent to which a user can manipulate a specific market.

For instance, with a 50x leverage position, an attacker would need to move the price by 0.2% to trigger liquidations. If the cap prevents a single position from influencing the OI by 0.2% or more, it would significantly hinder a user's ability to force others into liquidation.

By implementing such a cap, the protocol can reduce the impact of manipulation and enhance the stability and fairness of the market.

#### Resolution:

PariFi Team: Acknowledged, this is a known side effect of price impact that cannot be avoided gracefully.



### <a id="M15"></a> M-15 |  Order can fail becasue fees are deducted between validation checks. 

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

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/c258e0e46d784f3fbd281b39b995e0d816de2987




### <a id="M16"></a> M-16 Gas compensation for relayer does not consider the initial transaction gas cost.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L170

#### Description:

The starting gas amount is determined by simply calling `gasleft()` at the beginning of the `execute` function.

However, this amount is not the actual start gas value that should be used as there are 21000 gas required initially to send any transaction plus the gas paid for each byte of the calldata (4 gas per 0 byte, 8 gas per non-zero byte).

#### Recommendation:

Consider adding 21k gas + the gas spent for bytes to the `gasSent` on [line 170](https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L170).

#### Resolution:

PariFi Team: The issue was resolved in the following commit:

https://github.com/Parifi/parifi-contracts-internal/commit/d9829462ea1d9bab5b1ac14f2b64f654c1c4e41d



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

