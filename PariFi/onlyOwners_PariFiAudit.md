![](https://hackmd.io/_uploads/Hyv4GdsYn.jpg)




__Audit Firm__: Guardian Defender

__Client Firm__: PariFi

__Prepared By__: @devScrooge, @0xkato, @ret2basic, @saksham  

__Delivery Date:__ April 20th, 2022

<br />

PariFi engaged Guardian Defender to review the security of its Smart Contract system. From the 27th of June to the 20th of July, a team of 4 auditors reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | PariFi                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | https://github.com/GuardianAudits/PariFiDefenderAudit/tree/main                             |
| Commit       | [835c98b](https://github.com/GuardianAudits/PariFiDefenderAudit/commit/835c98b66fecfe072750c400ba3ac1b0f6a07ebd) |


| Delivery Date     | 20th July       |
|-------------------|--------------------------------|
| Audit Methodology | Static Analysis, Manual Review |


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 1     | 1       | 0        | 0            | 0                  | 0        |
| [High](#High)        | 3     | 3       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 13     | 13       | 0        | 0            | 0                  | 0        |
| [Low](#Low)          | 18     | 18       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope

| ID | File      | SHA-1 Hash                              |
|---------|------------|---------------------------------------|
| DAF      | src/DataFabric.sol | 0c7dac9a47fbf5526afdb2fd81c43f9f777e22df |
| FEM      |  src/FeeManager.sol | a32a44376b3884793b312f6239c3318185b7e9a8 |
| MVA      |  src/MarketVault.sol | 0c101c6d286c18adf2a0016bef1174aebb3cbc4a |
| ODS     |  src/OrderDS.sol | 5af392c2c678b7a1f5516bca240db986e7eb9bc0 |
| OMA     |  src/OrderManager.sol | d6543e7289a1553568063542085b487d96bd0e3b |
| PFF     |  src/PariFiForwarder.sol | 977d0cc719633bdf3acccd458f092f10e16ad2ae |
| PFE     |  src/PriceFeed.sol | f4e25a29b3489c4ef29ab648b521c8fa6903e103 |
| RBA     |  src/RBAC.sol | 3e20b89665c246b82030aadce9a495e8f6c4d222 |

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
| [C-01](#C01)  | Malicious user can take advantage of the system and gain a large profit.                                     | Logic error                 | CRITICAL     | Pending |
| [H-01](#H01)  | Gas griefing attack.                                                        | Validation         | HIGH     | Pending |
| [H-02](#H02)  | A malicious user can DOS other users.                                                       | DOS         | HIGH     | Pending |
| [H-03](#H03)  | If a position is liquidated when lossInCollateral <= liquidationThreshold then lossInCollateral amount is stuck on the contract forever .                                                       | Loss of funds         | HIGH     | Pending |
| [M-01](#M01)  | Multisig can be set to address(0) and tokens get lost. | Validation   | MEDIUM   | Pending |
| [M-02](#M02)  | Users can be liquidated upon creating a position.                                                      | Logic error         | MEDIUM   | Pending |
| [M-03](#M03)  | Stock splits are not accounted for.                                                      | Logic error         | MEDIUM   | Pending |
| [M-04](#M04)  | DOS for retrieving the price of certain assets.                                                      | Logic error         | MEDIUM   | Pending |
| [M-05](#M05)  | Some ‘cap variables’ are not correctly managed.                                                      | Validation         | MEDIUM   | Pending |
| [M-06](#M06)  | Fees keep accumulating even if the protocol is paused.                                                      | Logic error         | MEDIUM   | Pending |
| [M-07](#M07)  | Users can decrease the entirety of their collateral leaving them unable to get their profits.                                                      | Logic error         | MEDIUM   | Pending |
| [M-08](#M08)  | `MarketVault` and `OrderManager` can be paused independently from each other.                                                      | Logic error         | MEDIUM   | Pending |
| [M-09](#M09)  | Missing checks on priceFeed.latestRoundData().                                                      | Missing chekcs         | MEDIUM   | Pending |
| [M-10](#M010)  | MAX_PRICE_DELAY_PRIMARY and MAX_PRICE_DELAY_SECONDARY are too large for checking stale prices.                                                      | Logic error         | MEDIUM   | Pending |
| [M-11](#M11)  | updatedAvgPrice doesnt allow users full control of decreasing their position.                                                      | Logic error         | MEDIUM   | Pending |
| [M-12](#M12)  | isProfit can be wrong if users are short.                                                      | Logic error         | MEDIUM   | Pending |
| [M-13](#M13)  | Users Would Get Liquidated But Can't Keep Their Position Healthy In A Paused State.                                                      | Logic error         | MEDIUM   | Pending |
| [L-01](#L01)  | ETH can get stuck in a contract.                                                                           | Loss of funds | LOW      | Pending |
| [L-02](#L02)  | Missing ZERO check for _minDelay.                                                                             | Missing checks          | LOW      | Pending |
| [L-03](#L03)  |  Important variables can be changed without 2 step process.                                                                              | Centralization risk | LOW      | Pending |
| [L-04](#L04)  | getMarketPriceChainlink and getTokenPriceChainlink assume that every pair with USD is in 8 decimals.                                                                             | Logic error          | LOW      | Pending |
| [L-05](#L05)  | Multiplication by two should use bit shifting.                                                                             | Calculations          | LOW      | Pending |
| [L-06](#L06)  | Functions guaranteed to revert when called by normal users can be marked payable.                                                                        | Design | LOW      | Pending |
| [L-07](#L07)  | Use solidity version 0.8.20 to gain gas boost.                                                                             | Solidity version          | LOW      | Pending |
| [L-08](#L08)  | State variables only set in the constructor should be declared immutable.                                                                              | Variables          | LOW      | Pending |
| [L-09](#L09)  | Setting the constructor to payable.                                                                              | Constructor | LOW      | Pending |
| [L-10](#L10)  | Use custom errors instead of require                                                                              | Errors          | LOW      | Pending |
| [L-11](#L11)  | Using bools for storage incurs overhead                                                                              | Variables          | LOW      | Pending |
| [L-12](#L12)  | Use assembly to check for address(0)                                                                              | Checks | LOW      | Pending |
| [L-13](#L13)  | Division by two should use bit shifting                                                                              | Calculations          | LOW      | Pending |
| [L-14](#L14)  | Structs can be packed into fewer storage slots.                                                                             | Variables          | LOW      | Pending |
| [L-15](#L15)  | Multiple address/id mappings can be combined into a single mapping of an address/id to a struct, where appropiate,                                                                           | Variables          | LOW      | Pending |
| [L-16](#L16)  | CEI Violation                                                                           | Logic          | LOW      | Pending |
| [L-17](#L17)  | Incorrect comment                                                                           | Comment          | LOW      | Pending |
| [L-18](#L18)  | Misleading Variable Name                                                                           | Variable name          | LOW      | Pending |




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



----


## <a id="High"></a> High

### <a id="H01"></a> H-01 Gas griefing attack

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L183-L186

#### Description:
In the `PariFiForwarder.sol` contract, there is a variable called `gasPremium` which is used to determine if the user has to pay the gas or the protocol pays for it. If it is set to 0, then the gas is paid by the protocol. The problem with this is that the `execute()` function is the one that calls the `_chargeGasFees` and also sends a `.call` tx to `MarketVault` or `OrderManager BUT` does NOT check the return value:

```solidity
(bool success, bytes memory returndata) = transaction.toAddress.call{
  gas: transaction.minGas,
  value: transaction.txValue
}(abi.encodePacked(transaction.txData, transaction.fromAddress));
```

This means that even if the `.call` tx fails, the transaction will not revert.
Imagine an scenario where `gasPremium` is set to 0, therefore the user is not paying for the gas (the protocol is paying for it) and the user calls the `execute` function with some parameters that make the `.call` fail but not revert (for example sending value, this will fail as there is not payable functions on the destination contracts). Nothing will be actually executed due to the `.call` tx but it wont be reverted so the protocol pays the fee. The user can do this lots of times to consume all the funds of the protocol for paying fees.

After the `.call` the `_chargeGasFees` function is called: 
```solidity
_chargeGasFees(feeToken, transaction.fromAddress, gasSent);
```
#### Recommendation:
Add this line after `.call()`:
```solidity
require(success);
```

#### Resolution:





### <a id="H02"> H-02 A malicious user can DOS other users </a>

###  Description:

A malicious user can front run another user and create an order with the same `orderId`.

A malicious user opens a position and then waits for other users to create orders front run their order with a decrease order with the same `orderId` as the user, this decrease order cant be executed. 

Since the malicious user already created the position they won't get charged fees for the decrease since it cant be executed, allowing the malicious user to grief for a small gas cost.

```
PoC


function test_FrontRun_With_Same_OrderId() public {
        uint256 collateralAmount = 10 ether;
        uint256 currentMarketPrice = 3000;
        bool isLong = true;

        uint256 marketMaxLeverage = orderManager.getMarket(ETH_MARKET_ID).maxLeverage;
        uint256 orderSize = collateralAmount * marketMaxLeverage / PRECISION_MULTIPLIER;
        uint256 openingFeeAmount = orderSize * DEFAULT_OPENING_FEE / MAX_FEE;

        collateralAmount += openingFeeAmount;

        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);

        deal(WETH, TRADER_1, collateralAmount);

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
            orderSize: orderSize,
            expectedPrice: 0,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });


        vm.startPrank(TRADER_1);

        ERC20(WETH).approve(address(orderManager), collateralAmount);


        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();

        // TRADER_1 opens a position
        _settleOrder(orderId, currentMarketPrice);

        orderId = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_2, isLong, 0);

        orderStruct = OrderDS.Order({
            orderId: orderId,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_1,
            orderType: OrderDS.OrderType.DECREASE_POSITION,
            isLong: isLong,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: collateralAmount / 2,
            orderSize: orderSize / 2,
            expectedPrice: currentMarketPrice * PRICE_FACTOR,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_1);

        // TRADER_1 sees that TRADER_2 wants to create a new position and frontruns 
        // TRADER_2s order and creates a decrease order with TRADER_2s orderId, 
        // this decrease order can never be executed, meaning TRADER_1 will
        // never pay the fee, effectively greifing TRADER_2 for a small gas cost
        orderManager.createOrder(orderStruct, address(0));

        vm.stopPrank();

        deal(WETH, TRADER_2, collateralAmount);

        bytes32 orderId2 = orderManager.getOrderId(ETH_MARKET_ID, ethVaultAddress, TRADER_2, isLong, 0);

        OrderDS.Order memory orderStruct2 = OrderDS.Order({
            orderId: orderId2,
            marketId: ETH_MARKET_ID,
            userAddress: TRADER_2,
            orderType: OrderDS.OrderType.OPEN_NEW_POSITION,
            isLong: isLong,
            isLimitOrder: false,
            deadline: 0,
            collateralAmount: collateralAmount,
            orderSize: orderSize,
            expectedPrice: 0,
            maxSlippage: DEFAULT_TRADE_SLIPPAGE
        });

        vm.startPrank(TRADER_2);
        ERC20(WETH).approve(address(orderManager), collateralAmount);

        // Reverts because TRADER_1 already created an order with that orderId
        vm.expectRevert(LibError.ExistingOrder.selector);
        orderManager.createOrder(orderStruct2, address(0));
        vm.stopPrank();

    }
```

###  Recommendation:
Consider creating the orderId when creating the order itself.

###  Resolution:

### <a id="H03"></a> H-03 If a position is liquidated when `lossInCollateral <= liquidationThreshold` then `lossInCollateral` amount is stuck on the contract forever

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L386-L426

#### Description:

This issue is related with the `_liquidate` function in `OrderManager`:

```solidity
function _liquidatePosition(bytes32 _positionId) internal {
        Position storage userPosition = openPositions[_positionId];
        _validateExistingPosition(_positionId);

        Market memory market = availableMarkets[userPosition.marketId];
        uint256 executionPrice = _verifyAndUpdatePrice(userPosition.marketId, userPosition.isLong);

        (uint256 pnlInCollateral, bool isProfit) = getProfitOrLossInCollateral(_positionId, executionPrice);

        if (isProfit) revert LibError.LiquidationErrorNoLoss();

        // Calculate fees - Liquidation fee + closing fee + accrued borrow fees
        uint256 totalFeesMarket = (market.liquidationFee + market.closingFee) * userPosition.positionSize / MAX_FEE;
        totalFeesMarket = totalFeesMarket + getAccruedBorrowFeesInMarket(userPosition.positionId);

        uint256 feesInCollateral = priceFeed.convertMarketToToken(market.marketId, totalFeesMarket, market.depositToken);
        uint256 lossInCollateral = pnlInCollateral + feesInCollateral;
        if (lossInCollateral > userPosition.collateralAmount) {
            lossInCollateral = userPosition.collateralAmount;
        }

        uint256 liquidationThreshold =
            (market.liquidationThreshold * userPosition.collateralAmount) / PRECISION_MULTIPLIER;

        if (lossInCollateral > liquidationThreshold) {
            IERC20(market.depositToken).safeTransfer(feeManager, lossInCollateral);
            IFeeManager(feeManager).distributeFees(market.depositToken);
        }

        // Transfer any remaining collateral to user
        uint256 userBalance;
        if (userPosition.collateralAmount > lossInCollateral) {
            userBalance = userPosition.collateralAmount - lossInCollateral;
            IERC20(market.depositToken).safeTransfer(userPosition.userAddress, userBalance);
        }

        dataFabric.updateMarketData(market.marketId, userPosition.positionSize, userPosition.isLong, false);

        delete openPositions[_positionId];
        emit PositionLiquidated(_positionId, userBalance);
    }
``` 

The only check for liquidate a position is if it not in profit: `if (isProfit) revert LibError.LiquidationErrorNoLoss();` 
So if the position is at loss but have not passed the liquidationThreshold, this means that `lossInCollateral > liquidationThreshold`, then the position is going to be liquidated but `lossInCollateral` amount is not going to get stuck in the contract.
Imagine the following scenario:
1. `market.liquidationThreshold` is set to 80% as in the tests.
2.  `liquidationThreshold` is 100 (to keep numbers simple).
3. `pnlInCollateral` is 70 and `isProfit` is false.
4. `lossInCollateral` is 80 (pnlInCollateral + fees).
Due to `isProfit` being false the position is liquidable, due to `lossInCollateral` being 80 and `liquidationThreshold` being 100 the following block is bypassed:

```solidity
if (lossInCollateral > liquidationThreshold) {
            IERC20(market.depositToken).safeTransfer(feeManager, lossInCollateral);
            IFeeManager(feeManager).distributeFees(market.depositToken);
        }
```

So `lossInCollateral` amount is not transferred. Then, if `userPosition.collateralAmount > lossInCollateral`, `userPosition.collateralAmount - lossInCollateral` is transferred back to the user.

```solidity
// Transfer any remaining collateral to user
        uint256 userBalance;
        if (userPosition.collateralAmount > lossInCollateral) {
            userBalance = userPosition.collateralAmount - lossInCollateral;
            IERC20(market.depositToken).safeTransfer(userPosition.userAddress, userBalance);
        }
```

After that the marketData is updated and the position is deleted. This means that `lossInCollateral` is not being transferred anywhere and as far as I have seen, there is no external functionalities to withdraw these funds or do something with them.

POC:
```
function test_Leftover_Balance_In_orderManager() public {
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

        assertTrue(orderManager.isPendingOrder(orderId), "order");


        _settleOrder(orderId, currentMarketPrice);

        bytes32 positionId = orderManager.getPositionId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong);
        assertTrue(orderManager.isValidPosition(positionId), "!position");

        currentMarketPrice = 2800;
        skip(1);

        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);
    
        bytes[] memory priceUpdateData = getPythUpdateData(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);

        vm.startPrank(KEEPER_ADDR);

        orderManager.liquidatePosition(positionId, priceUpdateData);

        vm.stopPrank();

        positionId = orderManager.getPositionId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong);
        assertFalse(orderManager.isValidPosition(positionId), "!position");

        // Check that the orderManager contract now as balance.
        assertEq(ERC20(WETH).balanceOf(address(orderManager)), 82511000000000000);

        // Check that the user has been liquidated
        assertEq(ERC20(WETH).balanceOf(TRADER_1), 917488000000000000);
    }
```

#### Recommendation:
Only liquidate a position when `lossInCollateral > liquidationThreshold`.

#### Resolution:



-----------------



## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Multisig can be set to address(0) and tokens get lost
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L70-L75
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L79-L90

#### Description:
In the RBAC.sol contract there is a function to update the `multisig` address which is set to ADMIN role and is able to perform certain actions due to onlyAdmin modifier in other contracts. The functions used for updating this multisig are these 2 (it is done in a 2-step proccess):

```solidity
/// @notice Propose a new multisig address to be updated
/// @dev A time delay of `MINIMUM_DELAY` must pass before the multisig is updated
/// @param _newMultisig Address of the new multisig
function proposeNewMultisig(address _newMultisig) external onlyRole(ADMIN) {
    if (_newMultisig == multisig) revert LibError.InvalidAddress();
    proposedMultisig = _newMultisig;
    proposedTimestamp = block.timestamp;
    emit NewMultisigProposed(_newMultisig);
}

/// @notice Updated the existing multisig to the propose  multisig address after `MINIMUM_DELAY`
/// @dev A time delay of `MINIMUM_DELAY` must pass before the multisig is updated
function updateMultisig() external onlyRole(ADMIN) {
    if (proposedTimestamp + MINIMUM_DELAY >= block.timestamp) revert LibError.InvalidTimestamp();

    _revokeRole(ADMIN, multisig);
    _grantRole(ADMIN, proposedMultisig);

    emit MultisigUpdated(multisig, proposedMultisig);

    multisig = proposedMultisig;
    proposedMultisig = address(0);
    proposedTimestamp = type(uint256).max;
}
```

When proposing a new multisig it is checked that it is not the same one that the current multisig but it is not checked that it is not address(0). If the described scenario takes place, the protocol ADMINS are not going to be able to change the multisig to a new address until `MINIMUM_DELAY` has passed.

This scenario can also lead to the loss of funds. In the `MarketVault.sol` contract there is a `inCaseTokensGetStuck()` function:

```solidity
function inCaseTokensGetStuck(address _token) external onlyAdmin {
    if (_token == asset()) revert LibError.InvalidAddress();

    address multisig = rbac.getRoleMember(rbac.ADMIN(), 0);
    SafeERC20.safeTransfer(IERC20(_token), multisig, IERC20(_token).balanceOf(address(this)));
}
```
If multisig is loaded as the zero address, the ERC20 tokens will be transferred to the zero address so they will be burned.

#### Recommendation:
Add the following line to the `proposeNewMultisig` function:
```solidity
if (_newMultisig == address(0)) revert LibError.InvalidAddress();
```

#### Resolution:

### <a id="M02"> M-02 Users can be liquidated upon creating a position </a>

###  Description:
A user with a long position can be liquidated upon creating a position.

The only check that is made to prevent a liquidation from taking place is the following check.

`if (isProfit) revert LibError.LiquidationErrorNoLoss();`

```
PoC

function test_immediate_liquidation() public {
        uint256 collateralAmount = 9 ether;
        uint256 currentMarketPrice = 3000;
        bool isLong = true;

        uint256 openingFeeAmount = collateralAmount * DEFAULT_OPENING_FEE / MAX_FEE;

        collateralAmount += openingFeeAmount;
        updatePriceChainlink(chainlinkEthFeed, currentMarketPrice);
        updatePythPrice(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);
        skip(1);
        console.log("Initial balance: ",ERC20(WETH).balanceOf(TRADER_1));

        deal(WETH, TRADER_1, collateralAmount);
        console.log("Dealt Tokens: ",ERC20(WETH).balanceOf(TRADER_1));

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

        assertTrue(orderManager.isPendingOrder(orderId), "order");

        _settleOrder(orderId, currentMarketPrice);

       bytes32 positionId = orderManager.getPositionId(ETH_MARKET_ID, ethVaultAddress, TRADER_1, isLong);
        assertTrue(orderManager.isValidPosition(positionId), "!position");


        bytes[] memory priceUpdateData = getPythUpdateData(pythAddress, PYTH_PRICE_ID_ETH, currentMarketPrice);

        vm.startPrank(KEEPER_ADDR);

        // TRADER_1 gets liquidated right after creating the position
        orderManager.liquidatePosition(positionId, priceUpdateData);
        
        console.log("Balance after liquidation: ",ERC20(WETH).balanceOf(TRADER_1));

        vm.stopPrank();
    }
```

###  Recommendation:

Implement the following check

`require(lossInCollateral >= liquidationThreshold, "!liquidatable")`

###  Resolution:

### <a id="M03"> M-03 Stock splits are not accounted for</a>

#### Description:
Among many things that can be supported are equities, great care should be taken in case of forward stock splits, where the price per share is reduced and the number of shares a user owns increased. 

Other scenarios include reverse stock splits, mergers, etc.

This can leave users with large losses and gains depending on direction.

#### Recommendation:
carefully monitor markets where such assets are present.

#### Resolution:

### <a id="M04"></a> M-04 DOS for retrieving the price of certain assets

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L137-L167


#### Description:
The `_getChainlinkPrice` function used in `PriceFeed` is used to get the price of the traded assets by the protocol and its defined in the following way: 

```solidity
/// @notice Returns the adjusted Chainlink price for feedAddress
    /// @param feedAddress Address of the price feed
    /// @return marketPriceUsd Price in USD with 8 decimals
    /// @return priceTimestamp Timestamp when this feed was last updated
    function _getChainlinkPrice(address feedAddress)
        internal
        view
        returns (uint256 marketPriceUsd, uint256 priceTimestamp)
    {
        _validateSequencerUptime();
        AggregatorV2V3Interface priceFeed = AggregatorV2V3Interface(feedAddress);
        if (priceFeed.decimals() != 8) revert LibError.IncompatiblePriceFeed();

        // prettier-ignore
        (
            /* uint80 roundID */
            ,
            int256 tokenPrice,
            /*uint startedAt*/
            ,
            uint256 lastUpdated,
            /*uint80 answeredInRound*/
        ) = priceFeed.latestRoundData();

        if (lastUpdated + MAX_PRICE_DELAY_SECONDARY <= block.timestamp) {
            revert LibError.PriceOutdated();
        }
        if (tokenPrice == 0) revert LibError.InvalidPrice();

        priceTimestamp = lastUpdated;

        // Safely cast the value of signed int256 to unsigned uint256 using SafeCast library
        // as all operations later are executed on uint256
        marketPriceUsd = SafeCast.toUint256(tokenPrice);
    }
```

The important part is here:
```solidity
// Safely cast the value of signed int256 to unsigned uint256 using SafeCast library
// as all operations later are executed on uint256
marketPriceUsd = SafeCast.toUint256(tokenPrice);
```

As you see they are assuming that every returned price is >= 0. The sponsor confirmed me that any asset can be traded in the protocol so they include assets like `Oil Futures` which historically has reached to negative prices. This would have being revert by the SafeCast.

Some resources related with this issue:
https://www.ig.com/en/news-and-trade-ideas/what-do-negative-oil-prices-mean--200507
https://stackoverflow.com/questions/67094903/anybody-knows-why-chainlinks-pricefeed-return-price-value-with-int-type-while

#### Recommendation:
We aware that valid prices that are < 0 can be returned by Chainlink and modify the function calls that reverts in this case such us this call: `marketPriceUsd = SafeCast.toUint256(tokenPrice);`



#### Resolution:


### <a id="M05"></a> M-05 Some 'cap variables' are not correctly managed


#### Description:
There are some 'cap variables', values used to be the maximum or minimum ones for some specific variables whose checks can be bypassed somehow.

1. `_fee` in `MarketVault.sol`:
There is a function in MarketVault to change the withdrawalFee:
```solidity
/// @notice Update withdrawal fees for Vault
/// @param _fee updated withdrawal fee
function updateWithdrawalFee(uint256 _fee) external onlyAdmin {
    if (_fee > WITHDRAWAL_FEE_CAP) revert LibError.MaxFee();
    withdrawalFee = _fee;
    emit WithdrawalFeeUpdated(_fee);
    }
```

However, the withdrawalFee is firstly set on the constructor and there is no mechanism for ensuring that the fee is <= WITHDRAWAL_FEE_CAP.

2. `MAX_FEE` in `OrderManager.sol`:
another case where fees can be set above upper limit. In `OrderManager`. There is a function to set a new market:

```solidity
function addNewMarket(bytes32 _marketId, Market calldata _newMarket) external onlyAdmin {
        Market memory market = availableMarkets[_marketId];
        if (market.marketAddress != address(0)) revert LibError.InvalidMarketAddress();
        if (market.isLive) revert LibError.MarketIsActive();

        if (_newMarket.marketId == bytes32(0)) revert LibError.InvalidMarketId();
        availableMarkets[_marketId] = _newMarket;

        emit MarketAdded(_marketId);
    }
```

This is an `onlyAdmin` function but there is no check for the `openingFee` and `closingFee`.  Everytime these fees are used to calculate something in the protocol, the operation is divided by `MAX_FEE` which is set to `10_000` this is done for using these fees as a % that is not wanted to be over 100% but if `openingFee` and `closingFee` are set > 10_000 then the fee charged will be > 100%.

3. `gasPremium ` in `PariFiForwarder.sol`:
In the `PariFiForwarder` contract, there is a variable called `gasPremium` which is used to determine if the user has to pay the gas or the protocol pays for it. If it is set to 0, it is paid by the protocol, if `gasPremium` is set to `10_000` the user pays the 100% of the gas price, if `gasPremium` is set to `9_000` the user is paying 90% of the total gas price, so the user is getting a 10% discount (confirmed by the sponsor).

There is a function used to change the value for `gasPremium`

```solidity
 /// @notice Sets the premium that is paid extra for the meta tx
/// @dev  Set to 0 for free meta tx(sponsored gas fees by protocol), 10_000 for no premium(i.e 1x of actual gas fees)
/// Less than 10,000 for gas rebates and more than 10,000 for a premium
/// @param _newGasPremium Updated value for gas premium
function setGasPremium(uint256 _newGasPremium) external onlyAdmin {
    gasPremium = _newGasPremium;  // @audit not checked if it is < 10_000 
    emit GasPremiumUpdated(_newGasPremium);
}
```

The function is not checking if `_newGasPremium` is set to a value higher than `MAX_FEE`, which is `10_000`. The expected behaviour is that the largest value for `gasPremium` is `MAX_FEE` so a check in this function to ensure this condition should be added.

#### Recommendation:
Add checks in the mentioned parts to ensure that the values for the variables are correctly set.



#### Resolution:

### <a id="M06"></a> M-06 Fees keep accumulating even if the protocol is paused

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

### <a id="M077"></a> M-07 Users can decrease the entirety of their collateral leaving them unable to get their profits
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

        assertTrue(orderManager.isPendingOrder(orderId), "order");


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

### <a id="M08"></a> M-08 `MarketVault` and `OrderManager` can be paused independently from each other.

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L247-L250
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L180-L183


#### Description:
The issue here is that `MarketVault` and `OrderManager` can be paused independently from each other, this means that one of them can be paused while the other is unpaused, this can lead to risky scenarios, such as:
If a position is 'inProfit' and the user decides to create a closeOrder tx when the `OrderManager.sol` contract is NOT PAUSED, the order can be correctly created but, the following scenario can take place:

1. `OrderManager` is NOT paused and `MarketVault` neither
2. User's submit a `createOrder` tx and the order is added to pendingOrders.
3. Admins decides for whatever reason to pause `MarketVault` contract but `OrderManager` is not paused.
4. Position is at profit an executes this part inside `_closePosition` function:
```solidity
if (isProfit) {
          userBalance = userPosition.collateralAmount + pnlInCollateral;
          IMarketVault(market.marketAddress).withdrawUserProfits(pnlInCollateral);
     }
```
5. Tx reverts due to `.withdrawUserProfits` from the `MarketVault` contract implements `whenNotPaused` modifier:
```solidity
function withdrawUserProfits(uint256 _profitAmount) external whenNotPaused onlyOrderManager {
        SafeERC20.safeTransfer(IERC20(asset()), orderManager, _profitAmount);
        emit WithdrawUserProfit(_profitAmount);
  }
```

So basically if the admins pause `MarketVault` contract not a single opened position can be closed with profits until the contract is unpaused again.
Even worse, a position which is in profit when `MarketVault` is paused can get into a liquidable state when `MarketVault` is unpaused and directly get liquidated.

#### Recommendation:
Implement a method for pausing both contracts at the same time and do not allow one of them to be unpaused while the other is paused.

#### Resolution:

### <a id="M09"></a> M-09 Missing checks on `priceFeed.latestRoundData()`

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L146-L160

#### Description:

In `PriceFeed._getChainlinkPrice()`, the check `answeredInRound < roundId` is missing:

```solidity
    function _getChainlinkPrice(address feedAddress)
        internal
        view
        returns (uint256 marketPriceUsd, uint256 priceTimestamp)
    {
        _validateSequencerUptime();
        AggregatorV2V3Interface priceFeed = AggregatorV2V3Interface(feedAddress);
        if (priceFeed.decimals() != 8) revert LibError.IncompatiblePriceFeed();

        // prettier-ignore
        (
            /* uint80 roundID */
            ,
            int256 tokenPrice,
            /*uint startedAt*/
            ,
            uint256 lastUpdated,
            /*uint80 answeredInRound*/
        ) = priceFeed.latestRoundData();

        if (lastUpdated + MAX_PRICE_DELAY_SECONDARY <= block.timestamp) {
            revert LibError.PriceOutdated();
        }
        if (tokenPrice == 0) revert LibError.InvalidPrice();

        priceTimestamp = lastUpdated;

        // Safely cast the value of signed int256 to unsigned uint256 using SafeCast library
        // as all operations later are executed on uint256
        marketPriceUsd = SafeCast.toUint256(tokenPrice);
    }
```

Without the check, stale price could be used and it will lead further computations into error states.

Also, the call to `priceFeed.latestRoundData()` should be wrapped in a try-catch block. This is needed because multisigs controlling the price feed can block access to price feeds at will. If such thing happens, the `_getChainlinkPrice()` function will go into DoS state.

#### Recommendation:

Add a check:

```solidity
require(answeredInRound < roundId, "Price is stale");
```

after `priceFeed.latestRoundData()`.

For the call to `priceFeed.latestRoundData()` itself, wrap it into a try-catch block.

#### Resolution:

### <a id="M10"></a> M-10 `MAX_PRICE_DELAY_PRIMARY` and `MAX_PRICE_DELAY_SECONDARY` are too large for checking stale prices

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L115

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L157-L159

#### Description:

In `PriceFeed.sol`, `MAX_PRICE_DELAY_PRIMARY` is set to 120 seconds, and `MAX_PRICE_DELAY_SECONDARY` is set to 31 minutes. Later on, these two constants are used to check if the price returned by the oracle is stale:

```solidity
if (pythPrice.publishTime + MAX_PRICE_DELAY_PRIMARY <= block.timestamp) revert LibError.PriceOutdated();
```

```solidity
        if (lastUpdated + MAX_PRICE_DELAY_SECONDARY <= block.timestamp) {
            revert LibError.PriceOutdated();
        }
```

These two constants are too large for checking stale prices. For example, ETH / USD pool on Pyth updates every 2 ~ 5 seconds. The same pool on Chainlink updates every 27 seconds. Both pools have fast heartbeats that the current two constants can't handle.

#### Recommendation:

Update `MAX_PRICE_DELAY_PRIMARY` and `MAX_PRICE_DELAY_SECONDARY` to smaller numbers to handle fast-heartbeat pools.

#### Resolution:

### <a id="M011"></a> M-011 updatedAvgPrice doesnt allow users full control of decreasing their position

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

        assertTrue(orderManager.isPendingOrder(orderId), "order");


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

### <a id="M12"></a> M-12 `isProfit` can be wrong if users are short

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L643-L663

#### Description:
To decide whether a user is in profit or not we do the following.


```     uint256 profitOrLoss;
        if (userPosition.isLong) {
            if (_price > userPosition.avgPrice) {
                // User position is profitable
                profitOrLoss = (_price - userPosition.avgPrice);
                isProfit = true;
            } else {
                // User position is at loss
                profitOrLoss = (userPosition.avgPrice - _price);
                isProfit = false;
            }
        } else {
            if (_price > userPosition.avgPrice) {
                // User position is at loss
                profitOrLoss = (_price - userPosition.avgPrice);
                isProfit = false;
            } else {
                // User position is profitable
                profitOrLoss = (userPosition.avgPrice - _price);
                isProfit = true;
            }
        }
```

If a user is short they will be in profit if they are even which should not be the case.


#### Recommendation:
Change the check to _price >= userPosition.avgPrice when users are short as shown below.


```     uint256 profitOrLoss;
        if (userPosition.isLong) {
            if (_price > userPosition.avgPrice) {
                // User position is profitable
                profitOrLoss = (_price - userPosition.avgPrice);
                isProfit = true;
            } else {
                // User position is at loss
                profitOrLoss = (userPosition.avgPrice - _price);
                isProfit = false;
            }
        } else {
            if (_price >= userPosition.avgPrice) {
                // User position is at loss
                profitOrLoss = (_price - userPosition.avgPrice);
                isProfit = false;
            } else {
                // User position is profitable
                profitOrLoss = (userPosition.avgPrice - _price);
                isProfit = true;
            }
        }
```

#### Resolution:

### M-13 Users Would Get Liquidated But Can't Keep Their Position Healthy In A Paused State

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L555
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L520

### Description:

Assume the contract (OrderManager) is in a paused state ->

1.) UserA's  position is in an  unhealthy state and all ready to get liquidated ,in order to keep the position healthy the user would be calling createOrder() function to modify his position and keep it healthy

```solidity

function createOrder(Order memory _order, address partner) external nonReentrant whenNotPaused {

```

2.) Since the function above i.e. createOrder has a whenNotPaused modifier , the call would fail as the contract is in a paused state.

3.) Liquidator(keeper) seeks the opportunity to liquidate and calls the liquidatePosition() function on the user's position 

```solidity
function liquidatePosition(bytes32 positionId, bytes[] calldata priceUpdateData) external nonReentrant onlyKeeper {
```

As this function does not have a whenNotPaused modifier , this call would excecute.

4.) The user was not able to keep his position healthy even when wanted to(had the resources) and got liquidated.

### Recommendation:

Let the user keep his position healthy in an unhealthy state or make all liquidations for unhealthy positions prior to pausing the contract.

-----------------


## <a id="Low"></a> Low


### <a id="L-01"></a> L-01 `ETH` can get stuck in a contract


https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L519
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L437-L441

#### Description:
The `PriceFeed.sol` contract implements a receive() function, therefore it can receive ETH. The received ether is used on the `updatePriceFeeds` to `pyth` inside the `updatePythPrice` function:

```solidity
  function updatePythPrice(bytes[] calldata priceUpdateData) public onlyKeeper {
    uint256 fee = pyth.getUpdateFee(priceUpdateData);
    pyth.updatePriceFeeds{value: fee}(priceUpdateData);
    emit PythPriceUpdated(fee);
}
```

The problem is that if the contract is not used anymore or if a very large amount of `ETH` is sent to the contract and it can not be consumed ever the `ETH` will be stuck in the contract because there is not a mechanism for withdrawing the `ETH`

#### Recommendation:
Implement a `withdraw` function only callable by admins that can withdraw `ETH` from the contract.

#### Resolution:

### <a id="L02"></a> L-02 Missing ZERO check for `_minDelay`


https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L37-L51

#### Description:
In `RBAC.sol`, the constructor misses zero check on `_minDelay`. More importantly:

```solidity
constructor(address _multisig, uint256 _minDelay) {
    multisig = _multisig;

    MINIMUM_DELAY = _minDelay;

    ...
}
```

where the deployer can technically set `MINIMUM_DELAY` to 0 since there is no zero check on `_minDelay`. Also, note that there is no way to update `MINIMUM_DELAY` since it is set to be immutable:

```solidity
uint256 public immutable MINIMUM_DELAY;
```

Once the contract gets deployed, `MINIMUM_DELAY` will stay as a constant indefinitely. Setting `MINIMUM_DELAY` to 0 does not make much sense with the expected functionality for this variable

#### Recommendation:
Add this line inside the constructor:
```solidity
if (_mindDelay === 0) revert;
```

#### Resolution:

### <a id="L03"></a> L-03 Important variables can be changed without 2 step process

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L187-L191
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L195-L199

#### Description:
In `MarketVault` there are some functions to directly change some important variables that directly affect to the amount of funds that the users are going to receive when withdrawing, such as:

```solidity
/// @notice Update withdrawal fees for Vault
/// @param _fee updated withdrawal fee
function updateWithdrawalFee(uint256 _fee) external onlyAdmin {
    if (_fee > WITHDRAWAL_FEE_CAP) revert LibError.MaxFee();
    withdrawalFee = _fee;
    emit WithdrawalFeeUpdated(_fee);
}

/// @notice Update the Fee Manager
/// @param _newFeeManager New Fee Manager address
function updateFeeManager(address _newFeeManager) external onlyAdmin {
    if (_newFeeManager == address(0)) revert LibError.ZeroAddress();
    feeManager = _newFeeManager;
    emit FeeManagerUpdated(_newFeeManager);
}
```

These are `onlyAdmin` functions but an scenario can happen where a user creates a tx for withdrawing funds and an Admin front-runs his tx (intentionally or UNINTENTIONALLY). If the admin changes the value for the withdrawalFee and pays more gas so that the admin tx is processed before, the user is going to receive less funds than expected.


#### Recommendation:
Implement a 2-step mechanism for updating this variable.




#### Resolution:

### <a id="L04"></a> L-04 Not all USD pools are 8-decimal

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/835c98b66fecfe072750c400ba3ac1b0f6a07ebd/src/PriceFeed.sol#L261

#### Description:

In `PriceFeed.getMarketPriceChainlink()`, the Natspec says:

```
/// @dev Returns the price of token in USD with 8 decimals which is default for all USD chainlink price feeds
```

In fact, the constant `FEED_DECIMALS = 8` is used based on this assumption. However, not all USD pools are 8-decimal. For example, the [AMP/USD pool](https://data.chain.link/ethereum/mainnet/crypto-usd/ampl-usd) is 18-decimal. Since there is outlier, there is no guarantee that the 8-decimal convention for USD pools will be strickly followed in the future.

#### Recommendation:

Use 8-decimal or 18-decimal depending on the pool's configuration.

#### Resolution:




### <a id="L05"></a> L-05 Multiplication by two should use bit shifting

#### Description:

`<x>` * 2 is the same as `<x> << 1`. While the compiler uses the SHL opcode to accomplish both, the version that uses multiplication
incurs an overhead of 20 gas due to JUMPs to and from a compiler utility function that introduces checks which can be avoided by
using unchecked {} around the division by two.
  
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L182
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L237
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L262  

### <a id="L06"></a> L-06 Functions guaranteed to revert when called by normal users can be marked payable
  
#### Description:
  
If a function modifier such as onlyOwner is used, the function will revert if a normal user tries to pay the function. Marking the function as payable will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided. The extra opcodes avoided are CALLVALUE(2),DUP1(3),ISZERO(3),PUSH2(3),JUMPI(10),PUSH1(3),DUP1(3),REVERT(0),JUMPDEST(1),POP(2), which costs an average of about 21 gas per call to the function, in addition to the extra deployment cost
  
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L249
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L276
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L284
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L291
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L300
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L308
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L316
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L324
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L332
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L340
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L216
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L211
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L203
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L195
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L187
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L180
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L107
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L117
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L126
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L36
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L877
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L872
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L865
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L856
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L848
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L831
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L820
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L805
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L555
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L546
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L256
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L248
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L238
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L230
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L224
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L218
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L211
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L514
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L504
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L495
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L481
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L456
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L466
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L446
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L437
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L58
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L70
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L63
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L79
  
  
### <a id="L07"></a> L-07 Use solidity version 0.8.20 to gain gas boost
  
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L216
  
  
### <a id="L08"></a> L-08 State variables only set in the constructor should be declared immutable
  
#### Description:
  
Avoids a Gsset (20000 gas) in the constructor, and replaces each Gwarmacces (100 gas) with a PUSH32 (3 gas).

https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L38
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L67
  
  
### <a id="L9"></a> L-9 Setting the constructor to payable
  
#### Description:
  
The msg.sender is the address of the account that initiated the current call. This is the address where the current (external) function call came from. It is possible to delegatecall() to another contract, which means that msg.sender will be the address of the current contract, not the original caller (as defined by CALLER in the yellow paper).\n\nThe tx.origin is the address of the account that sent the transaction, which started it all. It is never an account with code, and for any externally owned account, it is the same as msg.sender.
  
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L77
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/MarketVault.sol#L64
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/FeeManager.sol#L52
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L91
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L85
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L97
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/RBAC.sol#L37
  
  
### <a id="L10"></a> L-10 Use custom errors instead of require
  
#### Description:
  
Instead of using error strings, to reduce deployment and runtime cost, you should use custom errors. This would save both deployment and runtime cost.
  

### <a id="L11"></a> L-11 Using bools for storage incurs overhead
  
#### Description:
  
Use uint256(1) and uint256(2) for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from ‘false’ to ‘true’, after having been ‘true’ in the past
  
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/DataFabric.sol#L77
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L733
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L166
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L75

### <a id="L12"></a> L-12 Use assembly to check for address(0)
  
#### Description:
  
It is cheaper to use assembly for 0 address checks.
  
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L807
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L459
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PariFiForwarder.sol#L103
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L485
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/PriceFeed.sol#L470
  
### <a id="L13"></a> L-13 Division by two should use bit shifting
  
#### Description:
  
`<x> / 2` is the same as `<x> >> 1`. While the compiler uses the SHR opcode to accomplish both, the version that uses division incurs an overhead of 20 gas due to JUMPs to and from a compiler utility function that introduces checks which can be avoided by using unchecked {} around the division by two.
  
https://github.com/GuardianAudits/PariFiDefenderAudit/blob/main/src/OrderManager.sol#L866

### <a id="L14"></a> L-14 STRUCTS CAN BE PACKED INTO FEWER STORAGE SLOTS
  
#### Description:
  
Each slot saved can avoid an extra Gsset (20000 gas) for the first setting of the struct. Subsequent reads as well as writes have smaller gas savings

### <a id="L15"></a> L-15 MULTIPLE ADDRESS/ID MAPPINGS CAN BE COMBINED INTO A SINGLE MAPPING OF AN ADDRESS/ID TO A STRUCT, WHERE APPROPRIATE
  
#### Description:
  
Saves a storage slot for the mapping. Depending on the circumstances and sizes of types, can avoid a Gsset (20000 gas) per mapping combined.
Reads and subsequent writes can also be cheaper when a function requires both values and they both fit in the same storage slot. Finally,
if both fields are accessed in the same function, can save ~42 gas per access due to not having to recalculate the key’s keccak256 hash (Gkeccak256 - 30 gas) 
and that calculation’s associated stack operations.
  
  

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
