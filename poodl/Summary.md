![](https://user-images.githubusercontent.com/101481663/231196842-0594fd8d-9cc4-485e-96f4-d2c8a7718046.jpg)


__Audit Firm__: Solidity Lab

__Client Firm__: Poodl Tech

__Prepared By:__ Owen Thurm

<br />

__PoodlTech__ engaged Solidity Lab to review the security of its Smart Contract system. From __February  25th, 2023__ to __March 7th, 2023__, __6__ teams of 20 total auditors reviewed the source code in scope. All findings have been recorded in the following report.

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
| [Critical](#Critical)| 1     | 0       | 0        | 0            | 0                  | 1        |
| [High](#High)        | 2     | 0       | 0        | 0            | 0                  | 2        |
| [Medium](#Medium)    | 4     | 0       | 0        | 0            | 0                  | 4        |


# Audit Scope & Methodology

## Scope

| ID | File      | Checksum                              |
|---------|------------|---------------------------------------|
| A      | contracts/DividendPayingToken.sol |03754dd9bb6810afaeea9f89c43b6587a123bc485aa7383cebe1d89480fe7b12  |
| B      | contracts/Token.sol | 415199e53bce4ef12717900ca4750e5fbf53b3451f0e17702dae3f1a47ba6f76 |

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





# Findings & Resolutions

| ID      | Title                                                                                     | Category            | Severity | Status  |
|-------|-------------------------------------------------------------------------------------------|---------------------|----------|---------|
| [C-01](#C01)  |Custom Token Users May Drain The Contract| Logical Error | CRITICAL | Resolved |
| [H-01](#H01)  |High Slippage | Slippage Protection| HIGH     | Resolved |
| [H-02](#H02)  |claimWait Can Be Circumvented| Logical Error | HIGH   | Resolved |
| [M-01](#M01)  |Potential Reentrancy| Reentrancy| MEDIUM   | Resolved |
| [M-02](#M02)  |Lost Dividends Upon Upgrade| Logical Error | MEDIUM   | Resolved |
| [M-03](#M03)  |Fees Can Overflow Cap| Logical Error| MEDIUM   | Resolved |
| [M-04](#M04)  |User Not Added Back To TokenHolders| Logical Error| MEDIUM   | Resolved |





## <a id="High"></a> Critical

### <a id="C01"></a> C-01 Custom Token Users May Drain The Contract

https://github.com/poodlTech/tokenAudit/blob/main/contracts/DividendPayingToken.sol#L131


#### Description:
If a user has a custom reward token then the  withdrawnDividends[user] is never updated.
The user is able to drain all the dividends.



#### Recommendation:
Move the L136 to L134

```solidity
  function _withdrawDividendOfUser(address payable user) internal returns (uint256) {
    uint256 _withdrawableDividend = withdrawableDividendOf(user);
    if (_withdrawableDividend > 0) {
        withdrawnDividends[user] = withdrawnDividends[user].add(_withdrawableDividend);
         // if no custom reward token send BNB.
        if(!userHasCustomRewardToken[user]){
          (bool success,) = user.call{value: _withdrawableDividend, gas: stipend}("");
          if(!success) {
            withdrawnDividends[user] = withdrawnDividends[user].sub(_withdrawableDividend);
            return 0;
          }
          emit DividendWithdrawn(user, _withdrawableDividend);
          return _withdrawableDividend;
        } else {  
          // if the reward is not BNB
          emit DividendWithdrawn(user, _withdrawableDividend);
          return swapETHForTokens(user, _withdrawableDividend);
        }
    }
    return 0;
  }
```




#### Resolution:

Poodl Team: The recommended fix was implemented.



-----------------


## <a id="Medium"></a> High

### <a id="H01"></a> H-01 High Slippage


https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/DividendPayingToken.sol#L169-L174
https://github.com/poodlTech/tokenAudit/blob/eebe267b3fdd75a82e09cc270b3c046b2c9f2c84/contracts/Token.sol#L388-L394



#### Description:

Both `swapTokensForETH()` and `swapETHForTokens()` calls to Uniswap V2 router are sandwichable. A malicious user can exploit the high slippage set by those calls to sandwich the user’s transaction and make profit.


#### Recommendation:

Set slippage accordingly or calculate through Uniswap V2 functions.



#### Resolution:

Poodl Team: The recommended fix was implemented.


### <a id="H02"></a> H-02 claimWait Can Be Circumvented


https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L256-L258
https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L260-L262


#### Description:

*DividendTracker* contract define `claimWait` to limit user claiming dividend, once per certain period of time.

But in *Token* contract, `claim` and `processAccount` are defined and calling *DividendTracker*'s `processAccount` without checking the `claimWait`.



#### Recommendation:

Check the eligibilty by calling `canAutoClaim` first. 

```
    function claim() external nonReentrant {
        // add checking if can claim this time
        require(dividendTracker.canAutoClaim(lastClaimTimes[msg.sender]), "you hit claim limit for time period")
        dividendTracker.processAccount(payable(msg.sender), false);
    }

    function processAccount(address account) external nonReentrant {
        // add checking if can claim this time
        require(dividendTracker.canAutoClaim(lastClaimTimes[account]), "you hit claim limit for time period")
        dividendTracker.processAccount(payable(account), false); 
    }
```

#### Resolution:

Poodl Team: The recommended fix was implemented.


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


#### Resolution:

Poodl Team: Acknowledged.



### <a id="M02"></a> M-02 Lost Dividends Upon Upgrade

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L113

#### Description:
When the Dividend Tracker is updated. The balance of dividends for all the user is equal to 0.

#### Recommendation:
Create a function in order to setBalance(payable(users[i]),balanceOf(users[i])) for all the user who has n amount of Token.
```solidity=
function setAllBalance(address[] calldata holders) external onlyOwner {
        for(uint256 i = 0; i < holders.length; i++) {
            dividendTracker.setBalance(payable(holders[i]), balanceOf(holders[i]));
        }
   }
```

#### Resolution:

Poodl Team: Acknowledged.


### <a id="M03"></a> M-03 Fees Can Overflow Cap

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L217-L220

#### Description:
In the `setSellTopUp` function, there’s a check that ensures that the `sellTopUp` fee plus `rewardsFee`, `marketingFee` and `liquidityFee` is never above `capFees`.

But the problem arises that in the `setMarketingFee`, `setLiquidityFee` and `setRewardsFee` functions don’t check for the `sellTopUp` fee.

So first we could set the `sellTopUp` to 15, and later set the other fee variables that’ll sum 15. So finally the `sellTopUp`, `rewardsFee`, `marketingFee` and `liquidityFee` will add 30.

#### Recommendation:
It is recommended to always validate if the fee are under the `capFees`, consider creating only one function to set all the fees.

#### Resolution:

Poodl Team: The suggested fix was implemented.



#### Resolution:

### <a id="M04"></a> M-04 User Not Added Back To TokenHolders

https://github.com/poodlTech/tokenAudit/blob/main/contracts/Token.sol#L533-L545

#### Description:
By removing and user from dividends it is removed from `tokenHoldersMap`, if the same user is included again in dividends, he is not added to map.

#### Recommendation:
It is recommended to include again the user in `tokenHoldersMap` in `includeInDividends` function.
 
#### Resolution:

Poodl Team: The suggested fix was implemented.

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
