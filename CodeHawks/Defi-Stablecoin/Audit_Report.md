![](https://i.imgur.com/CbozOoX.png)

### Table of Contents

- [1. About Maroutis](#1.-About-Maroutis)
- [2. Disclaimer](#2.-Disclaimer)
- [3. Risk Classification](#3.-Risk-Classification)
  - [Impact](#3.1-Impact)
  - [Likelihood](#3.2-Likelihood)
  - [Action Required for Severity Issues](#3.3-Action-Required)
- [4. Executive Summary](#4.-Executive-Summary)
  - [Summary](#4.1-Summary)
  - [Issues Found](#4.2-Issues-Found)
  - [Qualitative Analysis](#4.3-Qualitative-Analysis)
  - [Decentralization Score](#4.4-Decentralization-Score)
- [5. Scope](#5.-Scope)
- [6. Roles](#6.-Roles)
  - [Roles](#6.1-Roles)
- [7. System Overview](#7.-System-Overview)
- [8. Findings](#8.-Findings)
  - [High](#8.2.-High)
  - [Medium](#8.3-Medium)
  - [Low](#8.4-Low)
  - [Gas Optimization](#8.5-Gas-Optimization)
  - [Informational](#8.6-Informational)
- [9. Additional Recommendations](#9.-Additional-Recommendations)
- [10. Risk Management](#10.-Risk-Management)
- [11. Conclusion](#11.-Conclusion)

## 1. About Maroutis

I am an independent Quantitative and smart contract security auditor and developer. With a background in Computer Science and Quantitative finance. I strive to provide thorough and quality service in every audit I partake in. To read more about me you may visit my [github](https://github.com/MMtis), [twitter](https://twitter.com/Maroutis), and/or [linkedin]().

## 2. Disclaimer

> A smart contract security review can never verify the complete absence of vulnerabilities. This is a time, resource and expertise bound effort where I try to find as many vulnerabilities as possible.
>
> This report is not, nor should be considered, an “endorsement” or “disapproval” of any particular project or team. This report is not, nor should be considered, an indication of the economics or value of any “product” or “asset” created by any team or project that contracts Maroutis to perform a security assessment. This report does not provide any warranty or guarantee regarding the absolute bug-free nature of the technology analyzed, nor do they provide any indication of the technologies proprietors, business, business model or legal compliance.
>
> This report should not be used in any way to make decisions around investment or involvement with any particular project. This report in no way provides investment advice, nor should be leveraged as investment advice of any sort. This report represents an extensive assessing process intending to help our customers increase the quality of their code while reducing the high level of risk presented by cryptographic tokens and blockchain technology.
>
> Blockchain technology and cryptographic assets present a high level of ongoing risk. Maroutis's position is that each company and individual are responsible for their own due diligence and continuous security. Maroutis’s goal is to help reduce the attack vectors and the high level of variance associated with utilizing new and consistently changing technologies, and in no way claims any guarantee of security or functionality of the technology we agree to analyze.
>
> The assessment services provided by Maroutis is subject to dependencies and under continuing
> development. You agree that your access and/or use, including but not limited to any services, reports, and materials, will be at your sole risk on an as-is, where-is, and as-available basis. Cryptographic tokens are emergent technologies and carry with them high levels of technical risk and uncertainty. The assessment reports could include false positives, false negatives, and other unpredictable results. The services may access, and depend upon, multiple layers of third-parties.
>
> Notice that smart contracts deployed on the blockchain are not resistant from internal/external exploit. Notice that active smart contract owner privileges constitute an elevated impact to any smart contract’s safety and security. Therefore, Maroutis does not guarantee the explicit security of the audited smart contract, regardless of the verdict.

## 3. Risk Classification

|        | High     | Medium | Low    |
| ------ | -------- | ------ | ------ |
| High   | Critical | High   | Medium |
| Medium | High     | Medium | Low    |
| Low    | Medium   | Low    | Low    |

### 3.1 Impact

- High
  - Leads to a significant material loss of assets in the protocol or significantly harms a group of users.
- Medium
  - Only a small amount of funds can be lost or a core functionality of the protocol is affected.
- Low
  - Any kind of unexpected behavior with some of the protocol's functionalities.

### 3.2 Likelihood

- High
  - Almost certain to happen, easy to perform, or not easy but highly incentivized.
- Medium
  - Only conditionally possible or incentivized, but still relatively incentivized.
- Low
  - Little incentive and would need extra ordinary events to occur for this to happen.

### 3.3 Action Required

- Critical
  - Must fix as soon as possible (if already deployed)
- High
  - Must fix
- Medium
  - Should fix
- Low
  - Could fix

## 4. Executive Summary

### 4.1 Issues Found

| Severity         | Count |
| ---------------- | ----- |
| Critical         | 0     |
| High             | 2     |
| Medium           | 1     |
| Low              | 2     |
| Gas Optimization | 1     |
| Informational    | 2     |
| Total            | 8     |

## 4.2 Qualitative Analysis

| Metric          | Rating    | Comment                                    |
| --------------- | --------- | ------------------------------------------ |
| Code Complexity | Excellent | Functionality is kept simple and organized |
| Documentation   | Moderate  | Functionalities are very well documented   |
| Best Practices  | Moderate  | Some best practices are implemeted         |

## 4.3 Decentralization Score

The rating represents how decentralized the protocol is. The rating system operates on a scale of 1 -10. 1 Being very centralized, 10 Being very decentralized.

| Rating |
| ------ |
| 8      |

## 5. Scope

| Files In Scope              | SLOC |
| --------------------------- | ---- |
| <i>Contracts: 2</i>         |      |
| OracleLib.sol               | 16   |
| DecentralizedStableCoin.sol | 29   |
| DSCEngine.sol               | 191  |
| <b>Total Files: 2</b>       | 236  |

## 6. Roles

### 6.1 Roles

- ## Owner (DSCEngine)
  -
- ## Token Owner
- ## User

## 7. System Overview

- OracleLib.sol

  - This contract provides a library of functions that fetch and manage the price feed data from various oracles.

- DecentralizedStableCoin.sol

  - This contract handles the creation (minting) and destruction (burning) of the stablecoin (DSC). It also handles the transferring of DSC and checks if transactions follow the protocol rules.

- DSCEngine.sol
  - This is the core contract of the protocol. It manages the positions of users, handles the deposit and withdrawal of collateral, and manages the minting and burning of DSC. It also handles liquidations.

## 8. Findings

---

---

### 8.2 High

---

## [H-01] Potential for an undercollateralized, unliquidated position to trigger a significant liquidity problem

## Summary

The `liquidate()` function allows users to liquidate either the entirety or a portion of the position, provided the collateral value of the position ranges between 200% and 110% of the DSC amount. However, once the value drops below 110% of the DSC amount, the position largely becomes immobile and cannot be liquidated.
Also, If a user decides to liquidate a fraction of a position, a part of the remaining debt becomes impossible to liquidate due to the 10% bonus. If `unliquidatable` positions like this accumulate, they could lead to a potential insolvency for the entire protocol, which can trigger a mass sell-off.

## Vulnerability Detail

In the current `liquidate()` function, a user can choose the amount of DSC and the collateral token they wish to liquidate. The debt to be liquidated (equivalent amount in collateral + 10% bonus) is transferred to the liquidator, resulting in the partially liquidated position almost always leading to an irrecoverable debt due to the incurred 10% loss.
`    uint256 totalCollateralToRedeem = tokenAmountFromDebtCovered + bonusCollateral;`

Once the collateral value falls below 110% of the debt value, the position cannot be fully repaid. In cases where a liquidator decides to repay 100% of the debt, the 10% bonus will cause the following line to revert:
`   s_collateralDeposited[from][tokenCollateralAddress] -= amountCollateral;`

Positions like this could potentially gradually accumulate and pose a systemic risk to the entire protocol and may lead to a "bank run" scenario where holders rush to redeem their stablecoins for collateral, further devaluing the stablecoin.

Also, in the case of a sudden drop in the value of the collateral tokens, the unsolvency risk becomes imminent as all the positions will be impacted at the same time.

## Impact

The complete or partial inability to liquidate positions can lead to systemic insolvency and could trigger a bank run scenario, where holders rush to redeem their stablecoins for collateral, causing further devaluation of the stablecoin.

## Tools Used

A thorough examination of the code base was performed to identify this issue.

## Recommendation

To prevent a sudden drop in collateral value and or `unliquidatable` positions, the following protective measures should be added:

1. Automate the total position's liquidation by selling the collateral for DSC when the collateral total value in USD approaches 110% of the total debt value. This can be achieved by auctioning/exchanging the tokens. Consider utilizing Chainlink keepers for automating this process.
2. Implement a stability fee to regulate the creation/holding of DSC, thereby managing its supply.
3. Create a backstop mechanism where additional liquidity is used to cover the remaining debt (only if the protocol becomes illiquid).
4. Use correlation as a risk metric when choosing the collateral basket (this will be further explained in the Risk Management section).

## [H-02] Flash loan attack is a possibility

## Summary

An individual can borrow a large amount of assets without collateral, exploit price volatility, and then repay the loan within the same transaction, profiting from the manipulated changes in asset prices.

## Vulnerability Detail

DSCEngine uses Chainlink oracles for pricing data, which are robust and less prone to manipulation due to their decentralized nature. However, even with decentralized price feeds, an attacker might still be able to manipulate prices in low-liquidity situations. Furthermore, the lack of safeguards for price volatility in DSCEngine presents additional risk, as the protocol may not be equipped to handle sudden and significant price swings.

For example:

1. A user gets a large loan of ETH. This can be done through a flash loan service.
2. They mint a large amount of DSC tokens by backing them with ETH using the function `depositCollateralAndMintDsc()`.
3. The act of minting a large amount of DSC tokens would theoretically increase the price of DSC since it follows the law of supply and demand. Usually, minting more tokens might not affect the price since the system will automatically adjust to maintain the peg. However, as the DSCEngine and dsc stablecoin gets introduced to the market, the liquidity will be low and volatility very high. This might cause a temporary price increase.
4. If the price of DSC has increased, the user could potentially use the DSC to take out a larger loan of ETH, as the collateral value is higher. They could then repay the initial ETH loan and, if there's any surplus, they could keep it as profit.

## Impact

If a flash loan attack were successful, the potential impact could be significant. The attacker could potentially profit from manipulated price discrepancies at the expense of other users, undermining the integrity of the platform and causing financial losses. Furthermore, the resulting loss of trust from the users could lead to a mass exit, further destabilizing the platform.

## Tools Used

A detailed review of the code base was conducted to identify this issue.

## Recommendation:

To guard against potential flash loan attacks, it is recommended that DSCEngine implement comprehensive security measures:

1. Implement Volatility Safeguards: These measures could include circuit breakers that halt trading during extreme price volatility or mechanisms to limit the size of trades relative to the total liquidity available.

2. Monitor For Unusual Activity: Regular monitoring can help to identify and halt potential attacks before they fully unfold.

3. Implement some algorithmic logic to limit against extreme price movements.

---

### 8.3 Medium

---

## [M-01] Collateral decimals are hardcoded to be 1e18 in `getUsdValue`

## Summary

The `getUsdValue` function anticipates the argument amount to have 18 decimals. However, this is not always the case. For example, USDT token has 6 decimals, which may result in a revert when a user tries to take debt, or, in worst case scenario, the user might get liquidated and lose 10% of his collateral.

## Vulnerability Detail

Currently, the return value of the function `getUsdValue()` will have the same decimals of the token amount (the price feed price will always have 8 decimals
since the base currency is USD).
Suppose a token has fewer than 18 decimals, let's say 12. In such a case, `getUsdValue()` would return an amount in 12 decimal places.

This amount is then used to calculate the health factor in the function `_calculateHealthFactor()` function.
`    return (collateralAdjustedForThreshold * 1e18) / totalDscMinted;`
We are comparing an amount that is in 12 decimal places `collateralAdjustedForThreshold` with an amount that has 18 decimals places `totalDscMinted`.
The result will also be in 12 decimal places, causing the function `_revertIfHealthFactorIsBroken` to revert:
`    if (userHealthFactor < MIN_HEALTH_FACTOR) {`
`            revert DSCEngine__BreaksHealthFactor(userHealthFactor);`
`        }`

Suppose a user deposits a large amount of collateral, such as 2 000 000 tokens (equivalent to 2 \* 10\*\*18), then mints 1 DSC. If the token price slightly decreases, his position could be opened for liquidation. A liquidator could then repay his debt and get an additional 10% (around 200 000 tokens).
`   uint256 totalCollateralToRedeem = tokenAmountFromDebtCovered + bonusCollateral;`

Working with hardcoded decimal values creates weakesses, unless the collateral addresses are hardcoded and known to have the default decimals.

## Impact

If a user deposits a large amount of collateral, his position could be opened for liquidation if the token price slightly decreases. This could result in a loss of approximately 10% of the collateral.

## Tools Used

A thorough review of the code base was conducted to identify this issue.

## Recommendation

The best way to fix this would be to use the ERC20 function `decimals()` wherever a calculation between token amounts takes places. This ensures the result has the same decimals as the debt token. Consider modifying the `getUsdValue` function as follows:
`   return ((uint256(price) * ADDITIONAL_FEED_PRECISION) * amount * dsc.decimals()) / PRECISION * 10 ** IERC20(token).decimals`

---

### 8.4 Low

---

## [L-01] `transfer` and `transferFrom` do not always have return values

For most tokens, transfer will either return true or revert, so there's no need to check for a true return value. However, for tokens like USDT, these functions do not return anything, causing the following line in the `depositCollateral` function to fail despite correct execution:
`   bool success = IERC20(tokenCollateralAddress).transferFrom(msg.sender, address(this), amountCollateral);`

## [L-02] Known vulnerabilities of ERC-20 token, contract `DecentralizedStableCoin.sol`

- Double withdrawal attack is possible. More details [here](https://docs.google.com/document/d/1YLPtQxZu1UAvO9cZ1O2RPXBbT0mooh4DYKjA_jp-RLM/edit)
- The contract lacks a proper transaction handling mechanism, particularly for contracts without a fallback mechanism. [WARNING!](https://gist.github.com/Dexaran/ddb3e89fe64bf2e06ed15fbd5679bd20) This vulnerability has led to significant losses in the past. More details [here](https://docs.google.com/document/d/1Feh5sP6oQL1-1NHi-X1dbgT3ch2WdhbXRevDN681Jv4/edit).

## Recommendation:

For the first issue, a robust solution is to reimplement the approve function by comparing current allowance for spender with the current Value:

```solidity
File: DecentralizedStableCoin.sol

function approve(address spender,
                uint256 _currentValue
                uint256 amount) public internal override returns (bool) {
        if(_allowances[owner][spender] == _currentValue){
            address owner = _msgSender();
            _approve(owner, spender, amount);
            return true;
        }
        else {
            return false;
        }
    }
```

For the second issue, add the following code to the transfer(\_to address, ...) function:

```solidity
File: DecentralizedStableCoin.sol

require( _to != address(this) );
```

---

### 8.5 Gas Optimization

## [G-01] Unnecessary Health Factor Check in `burnDsc` Function

The function `_revertIfHealthFactorIsBroken` within the burnDsc function is not required and can be removed. Burning tokens will allow a user to improve his health factor regardless of the amount chosen to repay. This removal will enhance the deployment cost by around 1800 gas units.
Furthermore, this check currently prevents users from repaying a fraction of their debt in the case of an undercollateralized position. If a user chooses to repay a fraction of the debt that does not increase their health factor above the `MIN_HEALTH_FACTOR`, it should still improve their health factor. However, invoking `_revertIfHealthFactorIsBroken` stops them from doing so, which is an undesirable side effect. Removing this check would not only optimize gas but also provide a better user experience by allowing more flexibility in debt repayment strategies.

---

### 8.6 Informational

---

## [I-01] Update to Latest Solidity Version 0.8.21

## Summary

The contracts are currently written in Solidity version 0.8.18.

## Vulnerability Details

Using an outdated version of Solidity can lead to missed improvements, bug fixes, and security enhancements.

## Impact

The system may lack the latest security patches and improvements.

## Tools Used

A detailed review of the code base was conducted to identify this issue.

## Recommendations

Upgrade to the latest version of Solidity, 0.8.21.

## [I-02] Zero-Address Checks absent in Constructor

_Instances (1)_:`

```solidity
File: DSEngine.sol

constructor(address[] memory tokenAddresses, address[] memory priceFeedAddresses, address dscAddress)

```

## Impact

Accidental use of zero-addresses could result in contract malfunction, hindering operation or causing data corruption.

## Recommendations

Adding appropriate zero-address checks for the tokenAddresses, priceFeedAddresses, and dscAddress parameters in the constructor function would enhance the robustness of the protocol.

---

## 9. Additional Recommendations

- Improving fuzz testing
  - Some additional handlers/conditions can be added to improve fuzz testing and reduce the number of contract reverts.
- A DAO governance system can be implemented for futur decisions regarding the project.

## 10. Risk Management

- To mitigate the risk of a solvency crisis, the protocol could encourage users to use uncorrelated assets as collateral. Assets like ETH and BTC have high correlation, and their prices tend to move in sync. Conversely, many other tokens like USDT, DAI, and PAXG have low correlation. Providing a curated list of well-chosen collateral assets could help users manage their risk and reduce the long-term volatility risk for the protocol.

## 11. Conclusion

This project is a well-constructed DeFi stablecoin protocol with considerable effort invested in maintaining code simplicity and following industry best practices. Despite this, the audit identified some areas that require attention to ensure the robustness and security of the protocol. These include the treatment of decimals, solvency risk, the implementation of zero-address checks, ERC20 transfers and the use of the latest solidity version. By addressing these issues and considering the provided recommendations, the protocol can significantly improve its overall security and robustness.
