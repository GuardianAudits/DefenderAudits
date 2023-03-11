![](https://i.imgur.com/zGzXYfT.jpg)


__Audit Firm__: Solidity Lab

__Client Firm__: Poodl

__Prepared By:__ Cryptor, Jonatas Martins, Santipu

__Delivery Date:__ March 7th, 2022

<br />

__Poodl__ engaged Solidity Lab to review the security of its Smart Contract system. From __February 26th, 2022__ to __March 7th, 2022__, a team of __3__ auditors reviewed the source code in scope. All findings have been recorded in the following report.

Notice that the examined smart contracts are not resistant to external/internal exploit. For a detailed understanding of risk severity, source code vulnerability, and potential attack vectors, refer to the complete audit report below.



# Project Overview

| Project Name | RaisinLabs                                                                                               |
|--------------|----------------------------------------------------------------------------------------------------------|
| Language     | Solidity                                                                                                 |
| Codebase     | [tokenAudit](https://github.com/poodlTech/tokenAudit/)                      |
| Commit       | [eebe26...9f2c84](https://github.com/poodlTech/tokenAudit/tree/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/) |


| Delivery Date     | March 7th, 2022       |
|-------------------|--------------------------------|
| Audit Methodology | Static Analysis, Manual Review |


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 1     | 1       | 0        | 0            | 0                  | 0        |
| [High](#High)        | 4     | 4       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 5     | 5       | 0        | 0            | 0                  | 0        |
| [Low](#Low)          | 30     | 30       | 0        | 0            | 0                  | 0        |

# Audit Scope & Methodology

## Scope

| ID File | SHA-1      | Checksum                              |
|---------|------------|---------------------------------------|
| TK      | Token.sol | 47242647ae17e7bcd48b9ba6839076ffaf7add3a25c448f43aa511a26fbf7318 |
| DP      | DividendPayingToken.sol|d4b180f6a62e550e17ac4ac5a3012dfbae85c02e8344c74ef44b3d4dfd21ab7d |
| PR      | Presale.sol| 08dc34b82c139f659f01f8433c5bdf3f377f7fca0807930822a9832233ed4bc2 |
|AR       | Airdropper.sol| fb586c32f5141845b6157869d944a37902565d92f1dbd6db4cdf0e0966475b5e |

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

| ID      | Title   | Category    | Severity    | Status
|-------|-----------|---------|----------|---------|
[C-01](#C01) | Uninitialised variables leads to DoS in various functions | DoS| CRITICAL | Pending |
[H-01](#H01) | Contract doesn't compile | Compilation| HIGH | Pending |
[H-02](#H02) | Loss of all dividends when updating dividend tracker| Logic | HIGH | Pending |
[H-03](#H03) | Unset `_marketingWalletAddress` leads to loss of funds |Initialization | HIGH | Pending |
[H-04](#H04) | Slippage control is not correctly implemented leading to loss of funds | Logic | HIGH | Pending |
[M-01](#M01) | Centralization Risk for trusted owners | Ownership | MEDIUM | Pending |
[M-02](#M02) | Missing check for custom token | Validation | MEDIUM | Pending |
[M-03](#M03) | Check from setSellTopUp can be circumvented | Validation | MEDIUM | Pending |
[M-04](#M04) | No check in low-level call | Validation | MEDIUM | Pending |
[M-05](#M05) | Excluding and including user from dividends removes him from holders list | Validation | MEDIUM | Pending |
[L-01](#L01) | Unused Contracts | Unused Code | LOW | Pending |
[L-02](#L02) | Unused Events | Unused Code | LOW | Pending |
[L-03](#L03) | Trapped ETH | Logic | LOW | Pending |
[L-04](#L04) | Unreachable code | Dead Code | LOW | Pending |
[L-05](#L05) | Two storage mappings can be deleted | Gas Optimization | LOW | Pending |
[L-06](#L06) | Unnecesary `_transfer` function | Unused Code | LOW | Pending |
[L-07](#L07) | `withdrawDividend` function can be removed | Unused Code | LOW | Pending |
[L-08](#L08) | Avoid use fixed gas fees | Gas Optimization | LOW | Pending |
[L-09](#L09) | Missing event emission | Event | LOW | Pending |
[L-10](#L10) | Use a more recent version of Solidity | Versioning | LOW | Pending |
[L-11](#L11) | Consider Two-Step Ownership Transfer | Ownership | LOW | Pending |
[L-12](#L12) | Multiple contracts in same file | Architecture | LOW | Pending |
[L-13](#L13) | No need to use safeMath Library in solidity v0.8.4 | Versioning | LOW | Pending |
[L-14](#L14) | Return values of `approve()` not checked | Validation | LOW | Pending |
[L-15](#L15) | Unsafe ERC20 operations | Logic | LOW | Pending |
[L-16](#L16) | Using bools for storage incurs overhead | Gas Optimization | LOW | Pending |
[L-17](#L17) | Cache array length outside of loop | Gas Optimization | LOW | Pending |
[L-18](#L18) | Use Custom Errors | Gas Optimization | LOW | Pending |
[L-19](#L19) | Don’t initialize variables with default value | Gas Optimization | LOW | Pending |
[L-20](#L20) | Long revert strings | Gas Optimization | LOW | Pending |
[L-21](#L21) | `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too) | Gas Optimization | LOW | Pending |
[L-22](#L22) | Splitting `require()` statements that use `&&` saves gas | Gas Optimization | LOW | Pending |
[L-23](#L23) | Use `!= 0` instead of `> 0` for unsigned integer comparison | Gas Optimization | LOW | Pending |
[L-24](#L24) | In constructor the `_marketingWalletAddress` is set to `address(0)` | Unused Code | LOW | Pending |
[L-25](#L25) | Useless set of initial `totalFees` | Unused Code | LOW | Pending |
[L-26](#L26) | New owner is not excluded from fees | Logic | LOW | Pending |
[L-27](#L27) | No checks if the account is already excluded from fees or blacklisted | Validation | LOW | Pending |
[L-28](#L28) | Old marketing wallets are excluded from fees | Logic | LOW | Pending |
[L-29](#L29) | Unused declared variables | Unused Code | LOW | Pending |
[L-30](#L30) | WalletFee should not be set when TotalFee = 0 | Logic | LOW | Pending |








## <a id="Critical"></a>Critical


### <a id="C01"></a> C-01 Uninitialised variables leads to DoS in various functions

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L40
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L38

#### Description:

The `Airdropper` and `Presale` contracts have empty constructors so there're variables that aren't initialised.

In `Airdropper`, the variable `newToken` is never initialised so the two most critical functions `airdrop` and `emergencyWithdraw` will always revert causing a DoS. 

In `Presale`, the variables `cap`, `token` and `exchangeRatio` are never initialised and that leads to a DoS in the functions `buyPresale`, `emergencyWithdraw` and `closeAirdrop`. 


#### Recommendation:
Initialise all the mentioned variables in the constructor to avoid the `Airdropper` and `Presale` contracts to be unusable. 

```diff
- constructor() {}
+ constructor(address _newToken) {
+    newToken = _newToken;
+ }
```
```diff
- constructor() {}
+ constructor(uint256 _cap, address _token, uint256 _exchangeRatio) {
+    cap = _cap;
+    token = _token;
+    exchangeRatio = _exchangeRatio;
+ }
```

#### Resolution:


----




### <a id="H01"></a> H-01 Contract doesn't compile

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L51

#### Description:
The`Presale` contract doesn't compile due to a missing `payable` modifier in the `buyPresale` function. 

#### Recommendation:
Add a `payable` modifier to the `buyPresale` function. 

```diff
- function buyPresale(uint256 value) public {
+ function buyPresale(uint256 value) public payable {
```

#### Resolution:


### <a id="H02"></a> H-02 Loss of all dividends when updating dividend tracker

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L113-L125
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L534

#### Description:
It's not possible to change the dividend tracker to a tracker that was already set before. So if a dividend tracker contains dividens that weren't sent to the user when the owner changes the dividend tracker all the dividends will be lost.

If the owner tries to set a dividend tracker that was set before, the transaction will revert resulting in the loss of unclaimed dividends contained in that tracker.

This happens because in `updateDividendTracker` there is a line that exclude the dividend tracker from dividends. Given that an address can't be excluded from dividends if it's already excluded, the transaction will revert.
```solidity
function updateDividendTracker(address newAddress) public onlyOwner {
    require(newAddress != address(dividendTracker), "");
    DividendTracker newDividendTracker = DividendTracker(payable(newAddress));
    require(newDividendTracker.owner() == address(this), "");
    // @audit the dividend tracker has to be excluded from dividends
    newDividendTracker.excludeFromDividends(address(newDividendTracker));
    /*
    ...
    */
}
```

```solidity
function excludeFromDividends(address account) external onlyOwner {
    // @audit an account can't be exluded from dividends if it's already excluded
    require(!excludedFromDividends[account],"");
    excludedFromDividends[account] = true;
    _setBalance(account, 0);
    tokenHoldersMap.remove(account);
    emit ExcludeFromDividends(account);
}
```


#### POC:
```solidity
function testDividendTrackerChange() public {
   token = new Token();
   DividendTracker tracker = new DividendTracker();
   tracker.transferOwnership(address(token));
   DividendTracker oldTracker = token.dividendTracker();
   // @audit first time changing to a new dividend tracker
   token.updateDividendTracker(address(tracker));
   // @audit if try to change back to oldTracker gets an error
   // all the funds in old tracker will be lost
   vm.expectRevert();
   token.updateDividendTracker(address(oldTracker));
}
```

#### Recommendation:
It is recommended to change the `excludeFromDividends` function to the following code:
```diff
function excludeFromDividends(address account) external onlyOwner {
-  require(!excludedFromDividends[account],"");
   excludedFromDividends[account] = true;
   _setBalance(account, 0);
   tokenHoldersMap.remove(account);
   emit ExcludeFromDividends(account);
}
```
If we remove that `require()`, a dividend tracker will be able to be set a second time and recover the unclaimed dividends.



#### Resolution:


### <a id="H03"></a> H-03 Unset `_marketingWalletAddress` leads to loss of funds


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L373
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L414
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L420

#### Description:

By default `_marketingWalletAddress` is `address(0)`, and this variable is used in some functions. These functions make low level call to `_marketingWalletAddress` and send some ether or transfer any tokens, if the marketing address is not set, it will lose all ETH or tokens.

#### POC
Consider the case that Token contract has some native tokens, the `_marketingWalletAddress` was forgot to be set and the owner wants to withdraw.

By calling `withdrawBNB` function all native tokens will be transferred to `_marketingWalletAddress == address(0)` and it will be lost.


#### Recommendation: 
It's is recommended to validate that the `_marketingWalletAddress` is different from `address(0)` before send any token.

```diff
function withdrawBNB() public onlyOwner nonReentrant {
+   require(_marketingWalletAddress != address(0));
    uint256 amountBNB = address(this).balance;
    (bool success, ) = payable(_marketingWalletAddress).call{value: amountBNB}("");
    require(success, "transfer failed");
}
```
```diff
function withdrawBep20(address token) public onlyOwner nonReentrant{
+   require(_marketingWalletAddress != address(0));
    require((IERC20(address(token)).balanceOf(address(this)))>0);
    IERC20(token).safeTransfer(payable(_marketingWalletAddress),IERC20(token).balanceOf(address(this)));
}
```
```diff
function swapBack(uint256 contractTokenBalance) internal {
    
    ...
    
+   require(_marketingWalletAddress != address(0));
    (success,) = address(_marketingWalletAddress).call{value: amountBNBMarketing}("");
    
    ...
    
}
```

#### Resolution:



### <a id="H04"></a> H-04 Slippage control is not correctly implemented leading to loss of funds


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L388-L394
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L397-L409


#### Description:
In function `swapTokensForEth` and `addLiquidity` there is no slippage protection, making it vulnerable to sanwich attacks, MEV exploits and may lead to significant loss of yield.



#### Recommendation: 
There are different ways to set the slippage. The first one is to let users determine the maximum slippage they're willing to take. The protocol front-end should set the recommended value for them. The second one is have a slippage control parameters that's set by the owner.

#### Resolution:

----------------


## <a id="Medium"></a> Medium

### <a id="M01"></a> M-01 Centralization Risk for trusted owners
All Contracts

#### Description:
Contracts have owners with privileged rights to perform admin tasks and need to be trusted to not perform malicious updates or drain funds, in this case the owner has too much power and can steal all assets

#### Recommendation:
It is recommended to use a multisig wallet as owner of the contracts. And even if a multisig wallet is used, it's encouraged to follow the principle of the least privilege.


#### Resolution:

### <a id="M02"></a> M-02 Missing check for custom token

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L135

#### Description:
In `_withdrawDividendOfUser` the contract checks if the user has a custom reward token before distributing the rewards but it doesn’t check if that custom token is still approved by the contract. 

In the docs it states _"Both tokens and AMMs need to be approved by the team"_. There’s a possibility that the contract first approve a custom token and then it revokes the approval. When the approval of a custom token is revoked, no users should be receiving the dividends in that token.

#### Recommendation:
In `_withdrawDividendOfUser` add a check to ensure that the custom token the user has is still approved. 
Original code: 
```solidity
if(!userHasCustomRewardToken[user]){ 
  ...
}
```
Risk mitigation: 
```solidity
// @audit If the user has a custom reward token but it's not approved now, 
// it should receive the dividends in BNB instead of the custom token
if(!userHasCustomRewardToken[user] || !approvedTokens[userCurrentRewardToken[user]]){
  ...
}
```

#### Resolution:

### <a id="M03"></a> M-03 Check from setSellTopUp can be circumvented

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L217-L220

#### Description:
In the `setSellTopUp` function, there’s a check that ensures that the `sellTopUp` fee plus `rewardsFee`, `marketingFee` and `liquidityFee` is never above `capFees`.

But the problem arises that in the `setMarketingFee`, `setLiquidityFee` and `setRewardsFee` functions don’t check for the `sellTopUp` fee.

So first we could set the `sellTopUp` to 15, and later set the other fee variables that’ll sum 15. So finally the `sellTopUp`, `rewardsFee`, `marketingFee` and `liquidityFee` will add 30.

#### Recommendation:
It is recommended to always validate if the fee are under the `capFees`, consider creating only one function to set all the fees.

#### Resolution:

### <a id="M04"></a> M-04 No check in low-level call

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L369-L376

#### Description:
In swapBack function there is 2 calls that send value to the dividendTracker and the marketingWalletAddress. In both calls there is no require statement that checks that the call is sent correctly. 

#### Recommendation:
It is recommended to always check the return in low-level calls.

#### Resolution:

### <a id="M05"></a> M-05 Excluding and including user from dividends removes him from holders list
 
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L533-L545

#### Description:
By removing and user from dividends it is removed from `tokenHoldersMap`, if the same user is included again in dividends, he is not added to map.

#### Recommendation:
It is recommended to include again the user in `tokenHoldersMap` in `includeInDividends` function.
 
#### Resolution:

-----------------


## <a id="Low"></a> Low


### <a id="L01"></a> L-01 Unused Contracts

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L5
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L7
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L10-L24
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L6
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L7
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L10-L24

#### Description: 
There are imported contracts that aren't used in the code. This happens in various instances. 

#### Recommendation:
The imported contracts that aren't used should be removed from the code to reduce code size, avoid confusions and improve readability. 

#### Resolution: 


### <a id="L02"></a> L-02 Unused Events

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L35-L38

#### Description:
There is an event in the `Airdropper` contract that is not emitted anywhere in the code. 

#### Recommendation:
The events that aren't emitted in the contract should be removed to reduce code size, avoid confusions and improve readability.

#### Resolution:


### <a id="L03"></a> L-03 Trapped ETH

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L42
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L108

#### Description:
In the `Airdropper` contract there's an unnecessary `receive` function and no method to withdraw ETH, so if by error any user sends ETH to that contract it will be stuck there forever. 

Also in the Token contract, there is an unnecessary `receive` function that is empty and doesn't do anything. If a user sends ETH to this contract directly by mistake, they will be claimed by the owner.

#### Recommendation:
Remove the `receive` function in the `Airdropper` contract. 
Remove the `receive` function in the `token` contract.

### <a id="L04"></a> L-04 Unreachable code

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L191-L196
#### Description:
In the `_transfer` function we have 3 lines of code that will never execute. These 3 lines are a copy-paste of the `_transfer` function of the original ERC1726.

#### Recommendation:
The unreachable code should be removed to avoid confusions and improve readability. 

#### Resolution:

### <a id="L05"></a> L-05 Two storage mappings can be deleted

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L48
https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L51

#### Description:
The mappings `userHasCustomRewardAMM` and `userHasCustomRewardToken` can be deleted because the mappings `userCurrentRewardToken` and `userCurrentRewardAMM` can do the same check.

To know if the user has a custom reward token or AMM the contract can check if the value of `userCurrentRewardToken` and `userCurrentRewardAMM` is address zero or not. 

#### Recommendation:
The mappings `userHasCustomRewardAMM` and `userHasCustomRewardToken` can be deleted to save a lot of gas and the checks done by this mappings can be perfomed by the `userCurrentRewardToken` and `userCurrentRewardAMM` mappings.

#### Resolution:

### <a id="L06"></a> L-06 Unnecesary `_transfer` function

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L524-L527

#### Description:
The `_transfer` function in the `DividendTracker` contract always `revert()` when called it overrides the `_transfer` function in `DividendPayingToken` that also always `revert()`.

```solidity
// DividendTracker#524
function _transfer(address, address, uint256) internal override pure {
    //Users can not transfer this token as it has to be 1:1 with the parent token.
    require(false, "");
}
```
The `_transfer` function above overrides the `_transfer` function below. 
```solidity
// DividendPayingToken#191
function _transfer(address from, address to, uint256 value) internal virtual override {
    require(false);
    int256 _magCorrection = magnifiedDividendPerShare.mul(value).toInt256Safe();
    magnifiedDividendCorrections[from] = magnifiedDividendCorrections[from].add(_magCorrection);
    magnifiedDividendCorrections[to] = magnifiedDividendCorrections[to].sub(_magCorrection);
}
```

#### Recommendation:
This `_transfer` function in `DividendTracker` can be removed to reduce code size, improve readability and avoid confusions. 

#### Resolution:


### <a id="L07"></a> L-07 `withdrawDividend` function can be removed

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L529

#### Description:
The `withdrawDividend` function in `DividendTracker` overrides `withdrawDividend.` in `DividendPayingToken`. 

The first function always reverts but the second one can be called from the `DividendPayingToken` contract directly and use it. So the first function can be removed as it’s useless. 

```solidity
// DividendTracker#529
function withdrawDividend() public override pure { 
    require(false, "Use the 'claim' function");
} 
```
The function above overrides the function below. The first one can be removed. 
```solidity
// DividendPayingToken#125
function withdrawDividend() external virtual override {
  _withdrawDividendOfUser(payable(msg.sender));
}
```

#### Recommendation:
This `withdrawDividend` function in `DividendTracker` can be removed to reduce code size, improve readability and avoid confusions. 

#### Resolution:


### <a id="L08"></a> L-08 Avoid use fixed gas fees

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L53

#### Description:
The `stipend` variable stores the gas fee to spend in the call to withdraw the dividends of the users. 

This is a bad idea because in times of high volatility the gas fees can increase to a point where the `stipend` is not enough to cover the gas in order to withdraw the dividends of the users and transactions could revert.

#### Recommendation:
Remove the `stipend` variable and opt by not limiting the gas sent to a call.

If the `stipend` variable is used to avoid reentrancy, it's a better practice using a reentrancy guard.

#### Resolution:



### <a id="L09"></a> L-09 Missing event emission

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L47-L56
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L51-L58
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L60-L65
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L72-L75

#### Description:
There are state-changing methods that should emit some events so that off-chain monitoring can be implemented.

#### Recommendation:
Make sure to emit a proper event in each state-changing method to follow best practices. 

#### Resolution:

### <a id="L10"></a> L-10 Use a more recent version of Solidity

All contracts

#### Description:
For security, it is best practice to use the latest solidity version.
For the security fix list in the versions;
https://github.com/ethereum/solidity/blob/develop/Changelog.md

#### Recommendation:
It is recommended to update solidity version to recent 

#### Resolution:

### <a id="L11"></a> L-11 Consider Two-Step Ownership Transfer

All contracts

#### Description:
Owner can call the `transferOwnership` function to transfer the ownership to a new address directly. As such, there is a risk that the ownership is transferred to an invalid address, thus causing the contract to be without a owner.

#### Recommendation:
Implement a two-step ownership transfer approach by using `Ownable2Step` from OpenZeppelin as opposed to `Ownable` as it gives you the security of not unintentionally sending the owner role to an address you do not control.

#### Resolution:


### <a id="L12"></a> L-12 Multiple contracts in same file

All contracts

#### Description:
Here is the list of instance of multiple contract declaration in same folder:
 - Declaration of `Reentrancy` abstract contract in multiple files
 - DividendTracker is declared inside `Token.sol`

#### Recommendation:
Do not create same contract in different files or multiple contracts in same file, you should create one contract per file.

#### Resolution:

### <a id="L13"></a> L-13 No need to use safeMath Library in solidity v0.8.4

All contracts

#### Description:
The safemath Library is normally used to prevent underflow and overflows when uint variable is out of range. However, solidity V0.8.4 already checks for underflow and overflow so there is no need to use SafeMath.

#### Recommendation:
Remove SafeMath Library


#### Resolution:

### <a id="L14"></a> L-14 Return values of `approve()` not checked

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L385

#### Description:
Not all IERC20 implementations revert when there’s a failure in `approve()`. The function signature has a boolean return value and they indicate errors that way instead. By not checking the return value, operations that should have marked as failed, may potentially go through without actually approving anything

#### Recommendation:
It is recommended to check the return value of approve.

#### Resolution:

### <a id="L15"></a> L-15 Unsafe ERC20 operations

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L67
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L56
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L68

#### Description:
There are many Weird `ERC20` Tokens that won't work correctly using the standard `IERC20` interface. For example, `IERC20(token).transferFrom()` and `IERC20(token).transfer()` will fail for some tokens as they may not conform to the standard `IERC20` interface.

#### Recommendation:
Consider using `SafeERC20` for `transferFrom`, `transfer` and `approve`.

#### Resolution:

### <a id="L16"></a> L-16 Using bools for storage incurs overhead

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L32
https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L48-L49
https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L51-L52
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L36
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L38
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L44
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L56-L57
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L512

#### Description:
Use `uint256(1)` and `uint256(2)` for true/false to avoid a Gwarmaccess (100 gas), and to avoid Gsset (20000 gas) when changing from `false` to `true`, after having been `true` in the past. See source.

#### Recommendation:
It is recommended to change from bool to uint256 in mappings and state variables. 

#### Resolution:

### <a id="L17"></a> L-17 Cache array length outside of loop

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L49
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L60
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L159
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L170

#### Description:
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

#### Recommendation:
It is recommended to cache the array length outisde of loop

#### Resolution:

### <a id="L18"></a> L-18 Use Custom Errors

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L19
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L51
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Presale.sol#L41
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L421
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L530


#### Description:
If not cached, the solidity compiler will always read the length of the array during each iteration. That is, if it is a storage array, this is an extra sload operation (100 additional extra gas for each iteration except for the first) and if it is a memory array, this is an extra mload operation (3 additional gas for each iteration except for the first).

#### Recommendation:
Source Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.

#### Resolution:
It is recommended to use custom errors, as this example:

```solidity
//Declare the error
error Unauthorized();

//Use the error
revert Unauthorized();
```

### <a id="L19"></a> L-19 Don’t initialize variables with default value

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L49
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L60
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L159
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L170
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L574
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L576-L577

#### Description:
It is not necessary to initialize variables with default values

#### Recommendation:
It is recommended to remove the initialization for default values
```diff
- uint256 value = 0
+ uint256 value;

- bool flag = false
+ bool flag;
```

#### Resolution:

### <a id="L20"></a> L-20 Long revert strings

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L51

#### Description:
Using string with more than 32 bytes consume more gas

#### Recommendation:
It is recommended to change the revert error string to less than 32bytes or create custom errors.


#### Resolution:


### <a id="L21"></a> L-21 `++i` costs less gas than `i++`, especially when it’s used in forloops (`--i`/`i--` too)

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L49
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Airdropper.sol#L60
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L159
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L170
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L580
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L587
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L590

#### Description:
`++i` can save 5 gas per loop.

#### Recommendation:
It is recommended to change from i++ to ++i and to save more gas you can use the `unchecked` block.

#### Resolution:

### <a id="L22"></a> L-22 Splitting `require()` statements that use `&&` saves gas

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L297
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L548
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L338

#### Description:
Require statements that use `&&` consume more gas than multiple require statements.

#### Recommendation:
It is recommended to split the require statements

#### Resolution:

### <a id="L23"></a> L-23 Use `!= 0` instead of `> 0` for unsigned integer comparison

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L114-L115
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L374
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L413
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L603
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L657


#### Description:
The `!= 0` consumes less gas than `> 0`

#### Recommendation:
It is recommended to use `!= 0` instead of `> 0`

#### Resolution:

### <a id="L24"></a> L-24 In constructor the `_marketingWalletAddress` is set to address(0)

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L98


#### Description:
The line 98, `excludeFromFees(_marketingWalletAddress, true)` is setting exclude from fees for address(0)

#### Recommendation:
It is recommended to either set `_marketingWalletAddress` in constructor before exclude from fees or remove this line.

#### Resolution:

### <a id="L25"></a> L-25 Useless set of initial `totalFees`

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L52


#### Description:
At the start of the contract, the variable totalFees is set to equal `rewardsFee + liquidityFee + marketingFee`.This calculation is not needed because the initial values of this variables are all zero.

#### Recommendation:
It is recommended to either remove this useless calculation or set the initial fees on the constructor.

#### Resolution:

### <a id="L26"></a> L-26 New owner is not excluded from fees

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol


#### Description:
In the constructor, the owner of the contract is excluded from fees but if the ownership is transferrred, the new owner won’t be excluded from fees.

#### Recommendation:
Override `transferOwnership` function to exclude the new owner from fees

#### Resolution:

### <a id="L27"></a> L-27 No checks if the account is already excluded from fees or blacklisted

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L158-L163
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L169-L173


#### Description:
In the `excludeFromFees` function, it checks if the account to be excluded from the fees is already excluded. This is okey but in the `excludeMultipleAccountsFromFees` the check is missing.

Also, the `blacklistAddress` and `blacklistMultipleAddresses` functions don’t have checks to ensure the account to be blacklisted is already blacklisted.

#### Recommendation:
It is recommended to check if the account is already excluded from the fee in `excludeMultipleAccountsFromFees` and check if address is already blacklisted in `blacklistMultipleAddresses`

#### Resolution:

### <a id="L28"></a> L-28 Old marketing wallets are excluded from fees

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L187-192


#### Description:
In the `setMarketingWallet` function, it excludes from fees to the new marketing wallet but the old marketing wallet will still be excluded from fees. 

#### Recommendation:
It is recommended to change the `setMarketingWallet` function, set that the old marketing wallet is now not excluded from fees. 

#### Resolution:

### <a id="L29"></a> L-29 Unused declared variables

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L355-357


#### Description:
The variable `path` is not used, it is redeclared inside `swapTokensForEth`

#### Recommendation:
It is recommended to remove this dead code to save gas

#### Resolution:

### <a id="L30"></a> L-30 Check should be made that Owner cannot set WalletFee is TotalFees = 0

https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L337-L342


#### Description:
If TotalFee = 0, then the function transfer will not incur any walletfees even if it is > 0

#### Recommendation:
Check should be made that Owner cannot set WalletFee is TotalFees = 0


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