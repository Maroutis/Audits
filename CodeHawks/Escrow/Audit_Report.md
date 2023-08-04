![](https://i.imgur.com/olDVahG.png)

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
- [6. System Overview](#6.-System-Overview)
- [7. Findings](#7.-Findings)
  - [High](#7.2.-High)
  - [Medium](#7.3-Medium)
  - [Low](#7.4-Low)
  - [Gas Optimization](#7.5-Gas-Optimization)
  - [Informational](#7.6-Informational)
- [8. Additional Recommendations](#8.-Additional-Recommendations)
- [9. Conclusion](#9.-Conclusion)

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
| Medium           | 2     |
| Low              | 1     |
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

| Files In Scope        | SLOC |
| --------------------- | ---- |
| <i>Contracts: 2</i>   |      |
| IEscrow.sol.sol       | 24   |
| EscrowFactory.sol     | 57   |
| Escrow.sol            | 109  |
| IEscrowFactory.sol    | 7    |
| <b>Total Files: 2</b> | 197  |

## 6. System Overview

- IEscrowFactory.sol

  - This is an interface that outlines the functionality expected from the EscrowFactory contract.

- IEscrow.sol

  - This is an interface that defines the functionalities expected from the Escrow contract.

- EscrowFactory.sol

  - This is a contract responsible for the creation of new Escrow contracts and enforcing certain parameters during their creation.

- Escrow.sol

  - This is the main Escrow contract handling the logic of the escrow process, including dispute resolution.

## 7. Findings

### 7.2 High

---

## [H-01] Potential for Man-in-the-Middle Attack

## Summary

A malicious user can impersonate a legitimate seller, manipulating the communication between the buyer and the actual seller. The attacker convinces the buyer to set up the escrow contract with the actual seller's address and a trusted arbiter, then impersonates the buyer to the seller. This allows the attacker to receive the product without paying, leaving the actual buyer at a loss when the seller initiates a dispute.

## Vulnerability Detail

This issue arises from a lack of safeguards against impersonation attacks within the system. An attacker can use this to trick buyers into creating escrow contracts for their benefit, leaving unsuspecting buyers at a loss. The scam works because the real seller, seeing a valid contract with their address, may provide the service or product to the malicious actor, mistaking them for the legitimate buyer.

## Recommendation

It is recommended to implement additional security measures to prevent such impersonation attacks. This could include requiring some form of user verification or authentication when initiating a contract. Further, a secure channel for communication and contract initiation can be established between the buyer and seller, potentially using cryptographic techniques to verify each other's identities before the transaction.

Additional safeguards such as education for users about potential scams, alerts for suspicious activity, and procedures for dispute resolution can also be useful in mitigating this type of risk.

## [H-02] Possibility of Unauthorized Direct Deployment of an Escrow Contract

## Summary

The design of the Escrow.sol contract allows for its direct deployment bypassing the EscrowFactory. This poses a potential risk as the EscrowFactory safeguards the process by mandating the buyer as the contract deployer and ensuring buyer's funds are sent to the contract.

## Vulnerability Detail

An ill-intentioned actor could exploit this by creating a contract, injecting a minimal amount of funds, and mimicking the deployment process of the factory. This would expose both the seller and the arbiter to unnecessary risk. Furthermore, the actor could maliciously attribute someone else's address as the buyer, thereby ensnaring unsuspecting users.
The presence of such rogue contracts could also be leveraged by bad actors to camouflage their illicit activities since these contracts are not managed by the factory.

## Recommendation

To mitigate this, a requirement could be added within the constructor to ensure that the `msg.sender` is the address of the Factory.

---

### 7.3 Medium

---

## [M-01] Absence of Access Control for Escrow Creation Function (spam contract = multiply risks)

## Summary

The `newEscrow()` function in the Factory contract can be invoked by any user to establish escrows for any seller or arbiter.

## Vulnerability Detail

The unrestricted access to the `newEscrow()` function could potentially lead to spam escrows. The absence of an owner verification mechanism or fees allows users to create escrows for arbitrary addresses.

## Recommendation

Consider incorporating owner validations on this function or charge a fee when external users create an escrow to deter spam. In this case, deducting a fee might be more appropriate measure.

## [M-02] Potential Issues with Arbiter's Address

## Summary

The contract currently doesn't enforce any restrictions or checks concerning the arbiter's address.

## Vulnerability Detail

The absence of a specified arbiter's address could potentially lead to deadlock situations where a malevolent buyer intentionally abstains from invoking `confirmReceipt()`. Furthermore, if the arbiter's address is identical to that of the seller or buyer, it could facilitate illicit activities such as funds theft or non-payment for services.

## Recommendation

It would be beneficial to enforce the arbiter's address to differ from those of the seller, buyer, and address(0x). Additionally, a trusted arbiter's address should be made known to both parties, allowing for a correctness check when the contract is created.

---

### 7.4 Low

## [L-01] Unintended tokens Locked in Contract

## Summary

Funds (Ether or tokens) could be locked in the contract if they are directly sent to the contract's address without a corresponding function call.

## Vulnerability Detail

In Ethereum, when Ether is sent to an address without data, it's equivalent to calling a fallback function on a contract located at that address. If the fallback function does not have the functionality to handle incoming Ether, the Ether will be locked in the contract. The same applies for tokens, if a transfer is called instead of the intended transferFrom or any other function, the tokens could be locked.

In this case, if someone mistakenly sends Ether directly to the `Escrow` or `Factory` addresses, these funds would be stuck in the contract without any way of retrieval.

## Recommendation

To mitigate this issue, consider implementing a function that allows the contract owner to retrieve any unintended Ether or tokens sent to the contract. However, this function should be used with caution, as it could introduce additional security risks. Therefore, it's best to add strict access control, like onlyOwner, and possibly even a time delay to this function.

---

### 7.5 Gas Optimization

## [G-01] Use `i_price` instead of querying balance in the function `resolveDispute()`

The contract variable `i_price` can be used to determine how much should be sent to each party instead of querying the balance of the contract. This change will save approximately 30000 gas units on deployment of the Escrow contracts and 1000 gas units per invocation of the `resolveDispute()` function.

```solidity
File: Escrow.sol

function resolveDispute(uint256 buyerAward) external onlyArbiter nonReentrant inState(State.Disputed) {
        uint256 totalFee = buyerAward + i_arbiterFee;
        if (totalFee > i_price) {
            revert Escrow__TotalFeeExceedsBalance(i_price, totalFee);
        }

        s_state = State.Resolved;
        emit Resolved(i_buyer, i_seller);

        if (buyerAward > 0) {
            i_tokenContract.safeTransfer(i_buyer, buyerAward);
        }
        if (i_arbiterFee > 0) {
            i_tokenContract.safeTransfer(i_arbiter, i_arbiterFee);
        }
        uint256 remainingBalance = i_price - totalFee;
        if (remainingBalance > 0) {
            i_tokenContract.safeTransfer(i_seller, remainingBalance);
        }
    }

```

### 7.6 Informational

---

## [I-01] Update to Latest Solidity Version 0.8.21

The contracts are currently written in Solidity version 0.8.18. To stay up-to-date with the latest improvements, bug fixes, and security enhancements, it's advisable to upgrade to the latest version, 0.8.21.

## [I-02] Better tracking of deployed contracts in the `Factory`

In order for any deployed contract address to be easily retrievable and checked, any new deployed contract address can be added to an empty array or a mapping(BUYER -> Escrow) inside the `factory` contract.

## 8. Additional Recommendations

- Incorporate fuzz testing to ensure the system's robustness against random and unexpected inputs.

## 9. Conclusion

The audit reveals certain potential vulnerabilities that could be exploited by malicious actors. By incorporating the recommended changes, the system would be more secure, reliable, and resistant against possible exploits.
