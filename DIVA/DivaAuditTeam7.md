![](https://i.imgur.com/BCKndbr.jpg)


__Audit Firm__: Solidity Lab

__Client Firm__: DIVA Protocol

__Prepared By:__ devScrooge (JMariadlcs) - Cryptor - Saksham

__Delivery Date:__ April 20th, 2023

<br />

__Client Firm__ engaged Solidity Lab to review the security of its Smart Contract system. From __the 26th of March__ to __the 20th of April__, a team of __3__ auditors reviewed the source code in scope. All findings have been recorded in the following report.

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


| Vulnerability Level | Total | Pending | Declined | Acknowledged | Partially Resolved | Resolved |
|---------------------|-------|---------|----------|--------------|--------------------|----------|
| [Critical](#Critical)| 1     | 1       | 0        | 0            | 0                  | 0        |
| [High](#High)        | 1     | 1       | 0        | 0            | 0                  | 0        |
| [Medium](#Medium)    | 3     | 3       | 0        | 0            | 0                  | 0        |
| [Low](#Low)          | 1     | 1       | 0        | 0            | 0                  | 0        |
| [Gas](#Gas)          | 11     | 11       | 0        | 0            | 0                  | 0        |


# Audit Scope & Methodology

## Scope

| ID | File      | Checksum                              |
|---------|------------|---------------------------------------|
| A      | diva-contracts/contracts/* |-  |
| B      | oracles/contracts/* | - |


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
| [Gas](#Gas)                 | Gas Optimization

# Findings & Resolutions

| ID      | Title                                                                                     | Category            | Severity | Status  |
|-------|-------------------------------------------------------------------------------------------|---------------------|----------|---------|
| [C-01](#C01)  | Centralization Risk in token supply can result in users being unable to remove DIVA owner                                     | Centralization                 | Critical     | Pending |
| [H-01](#H01)  | User Will Lose Ether Which Was Sent to The Diamond Contract                                     | Loss of funds                 | High     | Pending |
| [M-01](#M01)  | Voting for a different owner can become impossible                                     | Logic                 | Medium     | Pending |
| [M-02](#M02)  | No functionality to deal with unclaimed election can result in Front run attacks                                   | Documentation                 | Medium     | Pending |
| [M-03](#M03)  | `_getActualTakerFillableAmount` Will Return `_takerCollateralAmount - _offerInfo.takerFilledAmount` Even If The Order Is Not Fillable                                   | Logic | Medium | Pending 
| [L-01](#L01)  | Diamond facet upgrade                   | Logic            | Low     | Pending |





## <a id="Critical"></a>Critical


### <a id="C01"></a> C-01 Centralization Risk in token supply can result in users being unable to remove DIVA owner 

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/DIVAOwnershipMain.sol#L100-L114

#### Description:
The DIVA token has a fixed supply of 100m (verified by dev)

Shown here: 

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/DIVAToken.sol#L1-L15

Also consider the following code: 


```
// Confirm that `msg.sender` has strictly more support than the current owner
        if (_candidateToStakedAmount[msg.sender] <= _candidateToStakedAmount[_owner]) {
            revert InsufficientStakingSupport();
        }
```

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/DIVAOwnershipMain.sol#L111-L114

A user cannot start an election unless he/she has more staked token support than the current DIVAowner 

This presents some problems, as it is very possible that a malicious whale DIVA owner can own enough of the circulating supply so that if the DIVA owner staked his tokens, no one would be able to challenge him in an election.

Considering the distribution model of the DIVA token, this is feasible as the owner does not need to own 51% of the total supply to pull this off 





#### Recommendation:

Consider loosening the requirements of starting an election so that a whale can still lose their position even if they control most of the circulating supply 






#### Resolution:


----


## <a id="High"></a> High

### <a id="H01"></a> H-01 User Will Lose Ether Which Was Sent to The Diamond Contract

#### Description:

Ether sent by a user here https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/Diamond.sol#L191 would be locked/lost forever i.e. their is no functionality to withdraw the sent amount neither by the user nor the owner. 

#### Recommendation:

Since their is no use of native currency on the Diamond contract , consider removing the receive function altogether.


-----------------



## <a id="High"></a> Medium

### <a id="M01"></a> M-01  Voting for a different owner can become impossible 

https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipMain.sol


#### Description:
As stated in the documentation, the stake function is used to 'vote' for a new protocol owner, and the documentation specifies that the minimum staking period is 7 days. This means that if a user stakes, for example, 50 DIVA tokens, they can only retrieve them after 7 days. If they change their mind and no longer wish to vote for the owner. To verify that the minimum staking period has passed, line 150 implements a check with the formula: `uint _minStakingPeriodEnd = _voterToTimestampLastStake[msg.sender] + _minStakingPeriod`. However, suppose the same user stakes another 50 tokens, for example, 3 days after staking the initial 50 tokens. In that case, the mapping will update to a minimum unstaking period of 10 days from the initial staking date, instead of the required 7 days according to the protocol docs. So the user will not be able to unstake his first staked amount after 7 days but after 10.

#### Recommendation:
Implement a logic that tracks the days for “each stake” of each user.


#### Resolution:





## <a id="Medium"></a> Medium

### <a id="M02"></a> M-02  No functionality to deal with unclaimed election can result in Front run attacks

https://github.com/GuardianAudits/DivaAudit/blob/5d0c7f6d854b65c2f723ac6bceea6bbd4edef37e/diva-contracts/contracts/DIVAOwnershipMain.sol#L129-L146

#### Description:

Following the docs: 

``In theory, the second or third placed candidates could become protocol owners if the winner does not submit their
ownership claim during the respective period. In practice though, we anticipate the election winner to always submit their
ownership claim.``

However, there doesn't seem to be any functionality for second or third place candidates to become protocol owner. This means that in the event that ownership is not claimed, the claiming ownership will not work properly as stakers can front run each other to either pull back funds and/or add funds for a candidate


#### Recommendation:
Add logic to allow second and third place candidates to become protocol owners in the event that the first place candidates does not submit his claim

#### Resolution:



### <a id="M03"></a> M-03 `_getActualTakerFillableAmount` Will Return `_takerCollateralAmount - _offerInfo.takerFilledAmount` Even If The Order Is Not Fillable

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


## <a id="Low"></a> Low


### <a id="L01"></a> L-01 Diamond facet upgrade

#### Description
If during an upgrade calls are executed in multiple Ethereum transactions, users may be exposed to contracts that are upgraded only partially, i.e., some of the
functions are upgraded while others are not. This may result in unexpected inconsistencies.

#### Recommendation

We recommend upgrading the contracts in a single transaction, or making the fallback function pausable for the duration of an upgrade.






-----------------


## <a id="Informational"></a> Informational

### <a id="I01"></a> I-01 Violation Of Checks Effects Interation Pattern

#### Description:

Eventhough every CEI pattern violation does not gurantee
that there is a reentrancy possible , it is always a good coding practice follow the pattern. Here are the code snippets where it was violated(transfer was done before some state updates) ->

https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/libraries/LibDIVA.sol#L654-L663
 
and 

https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipMain.sol#L80-L94


### <a id="I02"></a> I-02 revokePendingFeesUpdate Won't Work When There Is Only One Element In The Array

It is not mentioned in the comments nor in the docs that in
the function revokePendingFeesUpdate  https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/facets/GovernanceFacet.sol#L243 
the gs.fees array would have atleast one value after deployment which can't be challenged , because if it is assumed that this is not the case , then at L260 we perform a pop action which would make the array empty(assuming only one element)and then L263 would always revert due to underflow. In short first element's fee update won't be revoked.

### <a id="I03"></a> I-03 Functions , Variables And Parameters In Snake Case

#### Description:

Use camel case for all functions, parameters and variables and snake case for constants.

https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipMain.sol#L58-L61

https://github.com/GuardianAudits/DivaAudit/blob/main/diva-contracts/contracts/DIVAOwnershipSecondary.sol#L36-L40


## Gas Optimizations


### <a name="GAS-1"></a>[G-1] For Operations that will not overflow, you could use unchecked


```solidity
File: ClaimFacet.sol


44:                 ++i;

44:                 ++i;

79:                 ++i;

79:                 ++i;

116:         _fs.claimableFeeAmount[_collateralToken][msg.sender] -= _amount;

117:         _fs.claimableFeeAmount[_collateralToken][_recipient] += _amount;

```


```solidity
File: DiamondLoupeFacet.sol



18:         for (uint256 i; i < numFacets; i++) {

18:         for (uint256 i; i < numFacets; i++) {

```

```solidity
File: EIP712AddFacet.sol



32:                 ++i;

32:                 ++i;

```

```solidity
File: EIP712CancelFacet.sol

4: import {ReentrancyGuard} from "@solidstate/contracts/utils/ReentrancyGuard.sol";



24:                 ++i;

24:                 ++i;

42:                 ++i;

42:                 ++i;

60:                 ++i;

60:                 ++i;

104:             _offerRemoveLiquidity.maker 

104:             _offerRemoveLiquidity.maker 

104:             _offerRemoveLiquidity.maker 

```

```solidity
File: EIP712CreateFacet.sol




47:                 ++i;

47:                 ++i;

106:                             collateralAmount: _makerFillAmount +

```

```solidity
File: EIP712RemoveFacet.sol



33:                 ++i;

33:                 ++i;

```





```solidity
File: GovernanceFacet.sol



29:         LibDIVAStorage.Fees memory _fees = gs.fees[gs.fees.length - 1];

38:         uint256 _startTime = block.timestamp + 60 days;

83:             .settlementPeriods[gs.settlementPeriods.length - 1];

95:         uint256 _startTime = block.timestamp + 60 days;

165:         uint256 _startTimeNewFallbackDataProvider = block.timestamp + 60 days;

200:         uint256 _startTimeNewTreasury = block.timestamp + 2 days;

218:         if (block.timestamp <= gs.pauseReturnCollateralUntil + 2 days)

220:         gs.pauseReturnCollateralUntil = block.timestamp + 8 days;

249:         LibDIVAStorage.Fees memory _fees = gs.fees[gs.fees.length - 1];

263:         LibDIVAStorage.Fees memory _previousFees = gs.fees[gs.fees.length - 1];

263:         LibDIVAStorage.Fees memory _previousFees = gs.fees[gs.fees.length - 1];

263:         LibDIVAStorage.Fees memory _previousFees = gs.fees[gs.fees.length - 1];

263:         LibDIVAStorage.Fees memory _previousFees = gs.fees[gs.fees.length - 1];

292:             .settlementPeriods[gs.settlementPeriods.length - 1];

311:             .settlementPeriods[gs.settlementPeriods.length - 1];

408:             if (_fee < 100000000000000) revert FeeBelowMinimum(); // 0.01% = 0.0001

408:             if (_fee < 100000000000000) revert FeeBelowMinimum(); // 0.01% = 0.0001

409:             if (_fee > 15000000000000000) revert FeeAboveMaximum(); // 1.5% = 0.015

409:             if (_fee > 15000000000000000) revert FeeAboveMaximum(); // 1.5% = 0.015

```

```solidity
File: LiquidityFacet.sol




68:                 ++i;

68:                 ++i;

92:                 ++i;

92:                 ++i;

```

```solidity
File: PoolFacet.sol


44:                 ++i;

44:                 ++i;

```

```solidity
File: SettlementFacet.sol




50:                 ++i;

50:                 ++i;

91:                 ++i;

91:                 ++i;

126:                 ++i;

126:                 ++i;

279:                 _pool.statusTimestamp + _settlementPeriods.challengePeriod

298:                 _pool.statusTimestamp + _settlementPeriods.reviewPeriod

324:                 _tokenPayoutAmount = _pool.payoutLong; // net of fees

324:                 _tokenPayoutAmount = _pool.payoutLong; // net of fees

328:                 _tokenPayoutAmount = _pool.payoutShort; // net of fees

328:                 _tokenPayoutAmount = _pool.payoutShort; // net of fees

334:             uint256 _amountToReturn = (_tokenPayoutAmount * _amount) /

334:             uint256 _amountToReturn = (_tokenPayoutAmount * _amount) /

335:                 (10**uint256(_decimals));

335:                 (10**uint256(_decimals));

376:                 _pool.statusTimestamp + _settlementPeriods.challengePeriod

394:                 _pool.statusTimestamp + _settlementPeriods.reviewPeriod

439:             uint256 submissionEndTime = _pool.expiryTime +

477:                 submissionEndTime + _settlementPeriods.fallbackSubmissionPeriod

510:             uint256 reviewEndTime = _pool.statusTimestamp +

```

```solidity
File: TipFacet.sol



47:                 ++i;

47:                 ++i;

70:         _fs.poolIdToTip[_poolId] += _amount;

```

### <a name="G-2"></a>[G-2] Don't initialize variables with default value

*Instances (15)*:
```solidity
File: ClaimFacet.sol

37:         for (uint256 i = 0; i < len; ) {

71:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: EIP712AddFacet.sol

25:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: EIP712CancelFacet.sol

21:         for (uint256 i = 0; i < len; ) {

39:         for (uint256 i = 0; i < len; ) {

57:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: EIP712CreateFacet.sol

37:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: EIP712RemoveFacet.sol

26:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: LiquidityFacet.sol

43:         for (uint256 i = 0; i < len; ) {

85:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: PoolFacet.sol

34:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: SettlementFacet.sol

41:         for (uint256 i = 0; i < len; ) {

82:         for (uint256 i = 0; i < len; ) {

118:         for (uint256 i = 0; i < len; ) {

```

```solidity
File: TipFacet.sol

39:         for (uint256 i = 0; i < len; ) {

```

### <a name="G-3"></a>[G-3] Functions guaranteed to revert when called by normal users can be marked `payable`
If a function modifier such as `onlyOwner` is used, the function will revert if a normal user tries to pay the function. Marking the function as `payable` will lower the gas cost for legitimate callers because the compiler will not include checks for whether a payment was provided.

*Instances (5)*:
```solidity
File: GovernanceFacet.sol

179:     function updateTreasury(address _newTreasury) external override onlyOwner {

210:     function pauseReturnCollateral() external override onlyOwner {

227:     function unpauseReturnCollateral() external override onlyOwner {

243:     function revokePendingFeesUpdate() external override onlyOwner {

377:     function revokePendingTreasuryUpdate() external override onlyOwner {

```

### <a name="G-4"></a>[G-4] `++i` costs less gas than `i++`, especially when it's used in `for`-loops (`--i`/`i--` too)
*Saves 5 gas per loop*

*Instances (1)*:
```solidity
File: DiamondLoupeFacet.sol

18:         for (uint256 i; i < numFacets; i++) {

```

### <a name="G-5"></a>[G-5] Use != 0 instead of > 0 for unsigned integer comparison

*Instances (1)*:
```solidity
File: GovernanceFacet.sol

406:         if (_fee > 0) {

```

### <a name="GAS-6"></a>[G-6] Internal functions only called once can be inlined

```solidity
File: DIVAOwnershipMain.sol: 238-241

```
Description: Internal functions only called once can be inlined
Not inlining costs 20 to 40 gas because of two extra JUMP instructions and additional stack operations needed for function calls.
Recommendation: Include the function logic instead where the function is called instead of creating a new function for this.

### <a name="GAS-7"></a>[G-7] Using getter functions consume more gas

```solidity
DIVAOwnershipMain.sol: 183-185
DIVAOwnershipMain.sol: 193-915
DIVAOwnershipMain.sol: 197-199
DIVAOwnershipMain.sol: 201-203
DIVAOwnershipMain.sol: 205-207
DIVAOwnershipMain.sol: 209-211
DIVAOwnershipMain.sol: 213-216
DIVAOwnershipMain.sol: 217-219
DIVAOwnershipMain.sol: 221-223
DIVAOwnershipMain.sol: 225-227
```
Description: Using getter functions consume more gas than calling to public variables.
Recommendation: Change private variables to public instead of having getter functions.

### <a name="G-8"></a>[G-8] <x> += <y> Costs More Gas

```solidity
DIVAOwnershipMain.sol:91
DIVAOwnershipMain.sol:94
DIVAOwnershipMain.sol:162
DIVAOwnershipMain.sol:163
DIVADevelopmentFund.sol: 89
DIVADevelopmentFund.sol: 223
DIVADevelopmentFund.sol: 98
```

Description: <x> += <y> Costs More Gas Than <x> = <x> + <y> For State Variables

### <a name="G-9"></a>[G-9] Use Custom Error Strings
  
```solidity  
DIVADevelopmentFund.sol: 102
DIVADevelopmentFund.sol: 123
Description: Instead of using error strings, to reduce deployment and runtime cost, you should use Custom Errors. This would save both deployment and runtime cost.
```


### <a name="G-10"></a>[G-10] ps Variable Can Be Inlined
  
```solidity
LiquidityFacet.sol:110 
```
  
Description: ps variable can be inlined on line 111.
    
### <a id="G-11"></a> [G-11] Use while loop instead of for loop




#### Description:

 Instead of writing the following for loop with the following syntax

`For (uint256 i = 0; i < len; )`

Replace it with 

`while (uint i < len)`

to save gas







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
