# CTF Blockchain Challenges

This repository collects blockchain challenges in CTFs and wargames.

These challenges are categorized by topic, but they are not ordered by difficulty or by recommendation.

Some challenges come with my exploits (e.g., [Ethernaut](src/Ethernaut/), [Paradigm CTF 2022](src/ParadigmCTF2022/)).

If there are any incorrect descriptions, I would appreciate it if you could let me know via issue or PR.

---

**Table of Contents**
- [Ethereum](#ethereum)
  - [Ethereum/contract basics](#ethereumcontract-basics)
  - [EVM puzzles](#evm-puzzles)
  - [Misuse of `tx.origin`](#misuse-of-txorigin)
  - [Weak sources of randomness from chain attributes](#weak-sources-of-randomness-from-chain-attributes)
  - [ERC-20 basics](#erc-20-basics)
  - [Storage overwrite by `delegatecall`](#storage-overwrite-by-delegatecall)
  - [Context mismatch in `delegatecall`](#context-mismatch-in-delegatecall)
  - [Integer overflow](#integer-overflow)
  - [Non-executable Ether transfers to a contract](#non-executable-ether-transfers-to-a-contract)
  - [Forced Ether transfer to a contract via `selfdestruct`](#forced-ether-transfer-to-a-contract-via-selfdestruct)
  - [Large gas consumption by a contract callee](#large-gas-consumption-by-a-contract-callee)
  - [Forgetting to set `view`/`pure` to interface and abstract contract functions](#forgetting-to-set-viewpure-to-interface-and-abstract-contract-functions)
  - [`view` functions that do not always return the same value](#view-functions-that-do-not-always-return-the-same-value)
  - [Mistakes in setting `storage` and `memory`](#mistakes-in-setting-storage-and-memory)
  - [Transaction tracing](#transaction-tracing)
  - [Reversing states](#reversing-states)
  - [Reversing transactions](#reversing-transactions)
  - [Reversing EVM bytecode](#reversing-evm-bytecode)
  - [EVM bytecode golf](#evm-bytecode-golf)
  - [Gas optimization](#gas-optimization)
  - [Re-entrancy attack](#re-entrancy-attack)
  - [Flash loan basics](#flash-loan-basics)
  - [Massive rights by executing flash loans during snapshots](#massive-rights-by-executing-flash-loans-during-snapshots)
  - [Bypassing repayments of push architecture flash loans](#bypassing-repayments-of-push-architecture-flash-loans)
  - [Bug in AMM price calculation algorithm](#bug-in-amm-price-calculation-algorithm)
  - [Attack using custom tokens](#attack-using-custom-tokens)
  - [Funds leakage due to oracle manipulation (without flash loans)](#funds-leakage-due-to-oracle-manipulation-without-flash-loans)
  - [Funds leakage due to oracle manipulation (with flash loans)](#funds-leakage-due-to-oracle-manipulation-with-flash-loans)
  - [Sandwich attack](#sandwich-attack)
  - [Recovery of a private key by the same-nonce attack](#recovery-of-a-private-key-by-the-same-nonce-attack)
  - [Brute-force address](#brute-force-address)
  - [Recovery of a public key](#recovery-of-a-public-key)
  - [Encryption and decryption in secp256k1](#encryption-and-decryption-in-secp256k1)
  - [Bypassing bot and taking an ERC-20 token owned by a wallet with a known private key](#bypassing-bot-and-taking-an-erc-20-token-owned-by-a-wallet-with-a-known-private-key)
  - [Claimable intermediate nodes of a Merkle tree](#claimable-intermediate-nodes-of-a-merkle-tree)
  - [Precompiled contracts](#precompiled-contracts)
  - [Faking errors](#faking-errors)
  - [Foundry cheatcodes](#foundry-cheatcodes)
  - [Front-running](#front-running)
  - [Head overflow bug in calldata tuple ABI-reencoding (< Solidity 0.8.16)](#head-overflow-bug-in-calldata-tuple-abi-reencoding--solidity-0816)
  - [Arbitrary storage overwriting by setting an array length to `2^256-1` (< Solidity 0.6.0)](#arbitrary-storage-overwriting-by-setting-an-array-length-to-2256-1--solidity-060)
  - [Constructor that is just a function by a typo (< Solidity 0.5.0)](#constructor-that-is-just-a-function-by-a-typo--solidity-050)
  - [Storage overwrite via uninitialized storage pointer (< Solidity 0.5.0)](#storage-overwrite-via-uninitialized-storage-pointer--solidity-050)
  - [Other ad-hoc vulnerabilities and methods](#other-ad-hoc-vulnerabilities-and-methods)
- [Bitcoin](#bitcoin)
  - [Bitcoin basics](#bitcoin-basics)
  - [Recovery of a private key by the same nonce attack](#recovery-of-a-private-key-by-the-same-nonce-attack-1)
  - [Bypassing PoW of other applications using Bitcoin's PoW database](#bypassing-pow-of-other-applications-using-bitcoins-pow-database)
- [Cairo](#cairo)
- [Solana](#solana)
- [Other blockchain-related](#other-blockchain-related)
  - [IPFS](#ipfs)

---

## Ethereum

Note:
- If an attack is only valid for a particular version of Solidity and not for the latest version, the version is noted at the end of the heading.
- To avoid notation fluctuations, EVM terms are avoided as much as possible and Solidity terms are used.

### Ethereum/contract basics
- These challenges can be solved if you know the basic mechanics of Ethereum, [the basic language specification of Solidity](https://docs.soliditylang.org/en/latest/), and the basic operation of contracts.

| Challenge                                                          | Note, Keywords         |
| ------------------------------------------------------------------ | ---------------------- |
| [Capture The Ether: Deploy a contract](src/CaptureTheEther/)       | faucet, wallet         |
| [Capture The Ether: Call me](src/CaptureTheEther/)                 | contract call          |
| [Capture The Ether: Choose a nickname](src/CaptureTheEther/)       | contract call          |
| [Capture The Ether: Guess the number](src/CaptureTheEther/)        | contract call          |
| [Capture The Ether: Guess the secret number](src/CaptureTheEther/) | `keccak256`            |
| [Ethernaut: 0. Hello Ethernaut](src/Ethernaut/)                    | contract call, ABI     |
| [Ethernaut: 1. Fallback](src/Ethernaut/)                           | receive Ether function |
| [Paradigm CTF 2021: Hello](src/ParadigmCTF2021/)                   | contract call          |
| [0x41414141 CTF: sanity-check](src/0x41414141CTF/)                 | contract call          |
| [Paradigm CTF 2022: RANDOM](src/ParadigmCTF2022/)                  | contract call          |
| [DownUnderCTF 2022: Solve Me](src/DownUnderCTF2022/)               |                        |

### EVM puzzles
- Puzzle challenges that can be solved by understanding the EVM specifications.
- No vulnerabilities are used to solve these challenges.

| Challenge                                                          | Note, Keywords                                                         |
| ------------------------------------------------------------------ | ---------------------------------------------------------------------- |
| [Capture The Ether: Guess the new number](src/CaptureTheEther/)    | `block.number`, `block.timestamp` (formerly: `now`)                    |
| Capture The Ether: Predict the block hash                          | `blockhash` (formerly: `block.blockhash`)                              |
| [Ethernaut: 13. Gatekeeper One](src/Ethernaut/)                    | `msg.sender != tx.origin`, `gasleft().mod(8191) == 0`, type conversion |
| [Ethernaut: 14. Gatekeeper Two](src/Ethernaut/)                    | `msg.sender != tx.origin`, `extcodesize` is 0                          |
| Cipher Shastra: Minion                                             | `msg.sender != tx.origin`, `extcodesize` is 0, `block.timestamp`       |
| SECCON Beginners CTF 2020: C4B                                     | `block.number`                                                         |
| [Paradigm CTF 2021: Babysandbox](src/ParadigmCTF2021/Babysandbox/) | `staticcall`, `call`, `delegatecall`, `extcodesize` is 0               |
| Paradigm CTF 2021: Lockbox                                         | `ecrecover`, `abi.encodePacked`, `msg.data.length`                     |
| [EthernautDAO: 6. (No Name)](src/EthernautDAO/NoName/)             | `block.number`, gas price war                                          |
| [fvictorio's EVM Puzzles](src/FvictorioEVMPuzzles/)                |                                                                        |
| [Huff Challenge: Challenge #3](src/HuffChallenge/)                 |                                                                        |
| [Paradigm CTF 2022: LOCKBOX2](src/ParadigmCTF2022/)                |                                                                        |
| [Paradigm CTF 2022: SOURCECODE](src/ParadigmCTF2022/)              | quine                                                                  |

### Misuse of `tx.origin`
- `tx.origin` refers to the address of the transaction publisher and should not be used as the address of the contract caller `msg.sender`.

| Challenge                                 | Note, Keywords |
| ----------------------------------------- | -------------- |
| [Ethernaut: 4. Telephone](src/Ethernaut/) |                |

### Weak sources of randomness from chain attributes
- Since bytecodes of contracts are publicly available, it is easy to predict pseudorandom numbers whose generation is completed on-chain (using only states, not off-chain data).
- It is equivalent to having all the parameters of a pseudorandom number generator exposed.
- If you want to use random numbers that are unpredictable to anyone, use a decentralized oracle with a random number function. For example, [Chainlink VRF](https://docs.chain.link/docs/chainlink-vrf/), which implements Verifiable Random Function (VRF).

| Challenge                                                 | Note, Keywords |
| --------------------------------------------------------- | -------------- |
| Capture The Ether: Predict the future                     |                |
| [Ethernaut: 3. Coin Flip](src/Ethernaut/)                 |                |
| [DownUnderCTF 2022: Crypto Casino](src/DownUnderCTF2022/) |                |

### ERC-20 basics
- These challenges can be solved with an understanding of the [ERC-20 token standard](https://eips.ethereum.org/EIPS/eip-20).

| Challenge                                                                | Note, Keywords                        |
| ------------------------------------------------------------------------ | ------------------------------------- |
| [Ethernaut: 15. Naught Coin](src/Ethernaut/)                             | `transfer`, `approve`, `transferFrom` |
| [Paradigm CTF 2021: Secure](src/ParadigmCTF2021)                         | WETH                                  |
| [DeFi-Security-Summit-Stanford: VToken](src/DeFiSecuritySummitStanford/) |                                       |

### Storage overwrite by `delegatecall`
- `delegatecall` is a potential source of vulnerability because the storage of the `delegatecall` caller contract can be overwritten by the called function.

| Challenge                                                                              | Note, Keywords                                                                                    |
| -------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------- |
| [Ethernaut: 6. Delegation](src/Ethernaut/)                                             |                                                                                                   |
| [Ethernaut: 16. Preservation](src/Ethernaut/)                                          |                                                                                                   |
| [Ethernaut: 24. Puzzle Wallet](src/Ethernaut/)                                         | proxy contract                                                                                    |
| [Ethernaut: 25. Motorbike](src/Ethernaut/)                                             | proxy contract, [EIP-1967: Standard Proxy Storage Slots](https://eips.ethereum.org/EIPS/eip-1967) |
| [DeFi-Security-Summit-Stanford: InSecureumLenderPool](src/DeFiSecuritySummitStanford/) | flash loan                                                                                        |

### Context mismatch in `delegatecall`
- Functions called in `delegatecall` are executed in the context of the `delegatecall` caller contract. 
- If the function does not carefully consider the context, a bug will be created.

| Challenge                                                 | Note, Keywords             |
| --------------------------------------------------------- | -------------------------- |
| [EthernautDAO: 3. CarMarket](src/EthernautDAO/CarMarket/) | Non-use of `address(this)` |

### Integer overflow
- For example, subtracting `1` from the value of a variable of `uint` type when the value is `0` causes an arithmetic overflow.
- Arithmetic overflow has been detected and reverted state since Solidity v0.8.0.
- Contracts written in earlier versions can be checked by using [the SafeMath library](https://github.com/OpenZeppelin/openzeppelin-contracts/blob/release-v3.4/contracts/math/SafeMath.sol).

| Challenge                                              | Note, Keywords |
| ------------------------------------------------------ | -------------- |
| [Capture The Ether: Token sale](src/CaptureTheEther/)  | multiplication |
| [Capture The Ether: Token whale](src/CaptureTheEther/) | subtraction    |
| [Ethernaut: 5. Token](src/Ethernaut/)                  | subtraction    |

### Non-executable Ether transfers to a contract
- Do not create a contract on the assumption that normal Ether transfer (`.send()` or `.transfer()`) can always be executed.
- If a destination is a contract and there is no receive Ether function or payable fallback function, Ether cannot be transferred.
- However, instead of the normal transfer functions, the `selfdestruct` described below can be used to force such a contract to transfer Ether.

| Challenge                                                                  | Note, Keywords |
| -------------------------------------------------------------------------- | -------------- |
| [Ethernaut: 9. King](src/Ethernaut/)                                       |                |
| [Project SEKAI CTF 2022: Random Song](src/ProjectSekaiCTF2022/RandomSong/) | Chainlink VRF  |

### Forced Ether transfer to a contract via `selfdestruct`
- If a contract does not have a receive Ether function and a payable fallback function, it is not guaranteed that Ether will not be received.
- When a contract executes `selfdestruct`, it can transfer its Ether to another contract or EOA, and this `selfdestruct` transfer can be forced even if the destination contract does not have the receive Ether function and the payable fallback function. 
- If the application is built on the assumption that the Ether is `0`, it could be a bug.

| Challenge                                                  | Note, Keywords   |
| ---------------------------------------------------------- | ---------------- |
| [Capture The Ether: Retirement fund](src/CaptureTheEther/) | integer overflow |
| [Ethernaut: 7. Force](src/Ethernaut/)                      |                  |

### Large gas consumption by a contract callee
- A large amount of gas can be consumed by loops and recursion in `call`, and there may not be enough gas for the rest of the process.
- Until Solidity v0.8.0, zero division and `assert(false)` could consume a lot of gas.

| Challenge                               | Note, Keywords |
| --------------------------------------- | -------------- |
| [Ethernaut: 20. Denial](src/Ethernaut/) |                |

### Forgetting to set `view`/`pure` to interface and abstract contract functions
- If you forget to set `view` or `pure` for a function and design your application under the assumption that the state will not change, it will be a bug.

| Challenge                                 | Note, Keywords |
| ----------------------------------------- | -------------- |
| [Ethernaut: 11. Elevator](src/Ethernaut/) |                |

### `view` functions that do not always return the same value
- Since `view` functions can read state, they can be conditionally branched based on state and do not necessarily return the same value.

| Challenge                             | Note, Keywords |
| ------------------------------------- | -------------- |
| [Ethernaut: 21. Shop](src/Ethernaut/) |                |

### Mistakes in setting `storage` and `memory`
- If `storage` and `memory` are not set properly, old values may be referenced, or overwriting may not occur, resulting in vulnerability.

| Challenge            | Note, Keywords                                                                                                  |
| -------------------- | --------------------------------------------------------------------------------------------------------------- |
| N1CTF 2021: BabyDefi | [Cover Protocol infinite minting](https://coverprotocol.medium.com/12-28-post-mortem-34c5f9f718d4) + flash loan |

### Transaction tracing
- Various information can be obtained just by following the flow of transaction processing.
- Blockchain explorers such as Etherscan are useful.

| Challenge                                 | Note, Keywords                    |
| ----------------------------------------- | --------------------------------- |
| [Ethernaut: 17. Recovery](src/Ethernaut/) | loss of deployed contract address |

### Reversing states 
- Since the state and the bytecodes of contracts are public, all variables, including private variables, are readable.
- Private variables are only guaranteed not to be directly readable by other contracts, but we, as an entity outside the blockchain, can read them.

| Challenge                                                          | Note, Keywords |
| ------------------------------------------------------------------ | -------------- |
| [Capture The Ether: Guess the random number](src/CaptureTheEther/) |                |
| [Ethernaut: 8. Vault](src/Ethernaut/)                              |                |
| [Ethernaut: 12. Privacy](src/Ethernaut/)                           |                |
| Cipher Shastra: Sherlock                                           |                |
| [0x41414141 CTF: secure enclave](src/0x41414141CTF/)               | log, storage   |
| [EthernautDAO: 1. PrivateData](src/EthernautDAO/PrivateData/)      |                |

### Reversing transactions
- Reversing the contents of a transaction or how the state has been changed by the transaction.

| Challenge                                                        | Note, Keywords |
| ---------------------------------------------------------------- | -------------- |
| [darkCTF: Secret Of The Contract](src/DarkCTF/)                  |                |
| [DownUnderCTF 2022: Secret and Ephemeral](src/DownUnderCTF2022/) |                |


### Reversing EVM bytecode
- Reversing a contract for which code is not given in whole or in part.
- Use decompilers (e.g., [panoramix](https://github.com/eveem-org/panoramix), [ethervm.io](https://ethervm.io/decompile)) and disassemblers (e.g., [ethersplay](https://github.com/crytic/ethersplay)).

| Challenge                                                       | Note, Keywords                  |
| --------------------------------------------------------------- | ------------------------------- |
| Incognito 2.0: Ez                                               | keep in plain text              |
| Real World CTF 3rd: Re:Montagy                                  | Jump Oriented Programming (JOP) |
| [0x41414141 CTF: crackme.sol](src/0x41414141CTF/)               | decompile                       |
| [0x41414141 CTF: Crypto Casino](src/0x41414141CTF/)             | bypass condition check          |
| Paradigm CTF 2021: Babyrev                                      |                                 |
| Paradigm CTF 2021: JOP                                          | Jump Oriented Programming (JOP) |
| 34C3 CTF: Chaingang                                             |                                 |
| Blaze CTF 2018: Smart? Contract                                 |                                 |
| DEF CON CTF Qualifier 2018: SAG?                                |                                 |
| pbctf 2020: pbcoin                                              |                                 |
| Paradigm CTF 2022: STEALING-SATS                                |                                 |
| Paradigm CTF 2022: ELECTRIC-SHEEP                               |                                 |
| Paradigm CTF 2022: FUN-REVERSING-CHALLENGE                      |                                 |
| [DownUnderCTF 2022: EVM Vault Mechanism](src/DownUnderCTF2022/) |                                 |
| [EKOPARTY CTF 2022: Byte](src/EkoPartyCTF2022/)                 | stack tracing                   |
| [EKOPARTY CTF 2022: SmartRev](src/EkoPartyCTF2022/)             | memory tracing                  |

### EVM bytecode golf
- These challenges have a limit on the length of the bytecode to be created.

| Challenge                                              | Note, Keywords                                                                                                 |
| ------------------------------------------------------ | -------------------------------------------------------------------------------------------------------------- |
| [Ethernaut: 18. MagicNumber](src/Ethernaut/)           |                                                                                                                |
| [Paradigm CTF 2021: Rever](src/ParadigmCTF2021/Rever/) | Palindrome detection. In addition, the code that inverts the bytecode must also be able to detect palindromes. |
| [Huff Challenge: Challenge #1](src/HuffChallenge/)     |                                                                                                                |

### Gas optimization
- These challenges have a limit on the gas to be consumed.

| Challenge                                          | Note, Keywords |
| -------------------------------------------------- | -------------- |
| [Huff Challenge: Challenge #2](src/HuffChallenge/) |                |

### Re-entrancy attack
- In case a function of contract `A` contains interaction with another contract `B` or Ether transfer to `B`, the control is temporarily transferred to `B`.
- Since `B` can call `A` in this control, it will be a bug if the design is based on the assumption that `A` is not called in the middle of the execution of that function.
- For example, when `B` executes the `withdraw` function to withdraw Ether deposited in `A`, the Ether transfer triggers a control shift to `B`, and during the `withdraw` function, `B` executes `A`'s `withdraw` function again. Even if the `withdraw` function is designed to prevent withdrawal of more than the limit if it is simply called twice, if the `withdraw` function is executed in the middle of the `withdraw` function, it may be designed to bypass the limit check.
- To prevent re-entrancy attacks, use the Checks-Effects-Interactions pattern.

| Challenge                                                                       | Note, Keywords             |
| ------------------------------------------------------------------------------- | -------------------------- |
| [Capture The Ether: Token bank](src/CaptureTheEther/)                           | ERC-223, `tokenFallback()` |
| [Ethernaut: 10. Re-entrancy](src/Ethernaut/)                                    | `call`                     |
| Paradigm CTF 2021: Yield Aggregator                                             |                            |
| HTB University CTF 2020 Quals: moneyHeist                                       |                            |
| [EthernautDAO: 4. VendingMachine](src/EthernautDAO/VendingMachine/)             | `call`                     |
| [DeFi-Security-Summit-Stanford: InsecureDexLP](src/DeFiSecuritySummitStanford/) | ERC-223, `tokenFallback()` |
| [MapleCTF 2022: maplebacoin](src/MapleCTF/)                                     |                            |

### Flash loan basics
- Flash loans are uncollateralised loans that allow the borrowing of an asset, as long as the borrowed assets are returned before the end of the transaction. The borrower can deal with the borrowed assets any way they want within the transaction.
- By making large asset moves, attacks can be made to snatch funds from DeFi applications or to gain large amounts of votes for participation in governance.
- A solution to attacks that use flash loans to corrupt oracle values is to use a decentralized oracle.

| Challenge                              | Note, Keywords                                                                                                                                                |
| -------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Damn Vulnerable DeFi: 1. Unstoppable   | Simple flash loan with a single token. Failure to send the token directly.                                                                                    |
| Damn Vulnerable DeFi: 2. Naivereceiver | The `flashLoan` function can specify a `borrower`, but the receiver side does not authenticate the TX sender, so the receiver's funds can be drained as a fee |
| Damn Vulnerable DeFi: 3. Truster       | The target of a call is made into the token and the token can be taken by approving it to oneself                                                             |
| Damn Vulnerable DeFi: 4. Sideentrance  | Flash loan that allows each user to make a deposit and a withdrawal. The deposit can be executed at no cost at the time of the flash loan.                    |

### Massive rights by executing flash loans during snapshots
- If the algorithm distributes some kind of rights using the token balance at the time of a snapshot, and if a malicious user transaction can trigger a snapshot, a flash loan can be used to obtain massive rights.
- A period of time to lock the token will avoid this attack.

| Challenge                            | Note, Keywords                                                       |
| ------------------------------------ | -------------------------------------------------------------------- |
| Damn Vulnerable DeFi: 5. Therewarder | Get reward tokens based on the deposited token balance.              |
| Damn Vulnerable DeFi: 6. Selfie      | Get voting power in governance based on the deposited token balance. |

### Bypassing repayments of push architecture flash loans
- There are two architectures of flash loans: push and pull, with push architectures represented by Uniswap and Aave v1 and pull architectures by Aave v2 and dYdX.
- [EIP-3156: Flash Loans](https://eips.ethereum.org/EIPS/eip-3156) is a pull architecture.

| Challenge                  | Note, Keywords                                                  |
| -------------------------- | --------------------------------------------------------------- |
| Paradigm CTF 2021: Upgrade | Bypass using the lending functionality implemented in the token |

### Bug in AMM price calculation algorithm
- A bug in the Automated Market Maker (AMM) price calculation algorithm allows a simple combination of trades to drain funds.

| Challenge                            | Note, Keywords |
| ------------------------------------ | -------------- |
| [Ethernaut: 22. Dex](src/Ethernaut/) |                |

### Attack using custom tokens
- The ability of a protocol to use arbitrary tokens is not in itself a bad thing, but it can be an attack vector.
- In addition, bugs in the whitelist design, which assumes that arbitrary tokens are not available, could cause funds to drain.

| Challenge                                | Note, Keywords |
| ---------------------------------------- | -------------- |
| [Ethernaut: 23. Dex Two](src/Ethernaut/) |                |

### Funds leakage due to oracle manipulation (without flash loans)
- It corrupts the value of the oracle and drains the funds of applications that refer to that oracle.

| Challenge                            | Note, Keywords                                                                                  |
| ------------------------------------ | ----------------------------------------------------------------------------------------------- |
| Paradigm CTF 2021: Broker            | Distort Uniswap prices and liquidate positions on lending platforms that reference those prices |
| Damn Vulnerable DeFi: 7. Compromised | Off-chain private key leak & oracle manipulation                                                |

### Funds leakage due to oracle manipulation (with flash loans)
- The use of flash loans distorts the value of the oracle and drains the funds of the protocols that reference that oracle.
- The ability to move large amounts of funds through a flash loan makes it easy to distort the oracle and cause more damage.

| Challenge                                                                                    | Note, Keywords                                                                                     |
| -------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------- |
| Damn Vulnerable DeFi: 8. Puppet                                                              | Distort the price of Uniswap V1 and leak tokens from a lending platform that references that price |
| [DeFi-Security-Summit-Stanford: BorrowSystemInsecureOracle](src/DeFiSecuritySummitStanford/) | lending protocol                                                                                   |

### Sandwich attack
- For example, if there is a transaction by another party to sell token `A` and buy `B`, the attacker can put in a transaction to sell `A` and buy `B` before the transaction, and later put in a transaction to sell the same amount of `B` and buy `A`, thereby ultimately increasing the amount of `A` at a profit.
- In general, such "revenue earned by selecting, inserting, and reordering transactions contained in a block generated by a miner" is referred to as Miner Extractable Value (MEV). Recently, it is also called Maximal Extractable Value.

| Challenge                                         | Note, Keywords                              |
| ------------------------------------------------- | ------------------------------------------- |
| [Paradigm CTF 2021: Farmer](src/ParadigmCTF2021/) | Sandwich the trade from COMP to WETH to DAI |


### Recovery of a private key by the same-nonce attack
- In general, the same-nonce attack is a possible attack when the same nonce is used for different messages in the elliptic curve DSA (ECDSA), and the secret key is calculated.
- In Ethereum, if nonces used to sign transactions are the same, this attack is feasible.

| Challenge                                            | Note, Keywords |
| ---------------------------------------------------- | -------------- |
| Capture The Ether: Account Takeover                  |                |
| [Paradigm CTF 2021: Babycrypto](src/ParadigmCTF2021) |                |

### Brute-force address
- Brute force can make the start and end of an address a specific value.

| Challenge                         | Note, Keywords |
| --------------------------------- | -------------- |
| Capture The Ether: Fuzzy identity |                |

### Recovery of a public key
- The address is the public key applied to a `keccak256` hash, and the public key cannot be recovered from the address.
- If even one transaction has been sent, the public key can be back-calculated from it.
- Specifically, it can be recovered from the value of `keccak256` applied to Recursive Length Prefix (RLP)-encoded data by serializing the transaction and the signature `(r,s,v)`.

| Challenge                     | Note, Keywords |
| ----------------------------- | -------------- |
| Capture The Ether: Public Key |                |

### Encryption and decryption in secp256k1

| Challenge                                       | Note, Keywords  |
| ----------------------------------------------- | --------------- |
| [0x41414141 CTF: Rich Club](src/0x41414141CTF/) | DEX, flash loan |

### Bypassing bot and taking an ERC-20 token owned by a wallet with a known private key
- If a wallet with a known private key has an ERC-20 token but no Ether, it is usually necessary to first send Ether to the wallet and then `transfer` the ERC-20 token to get the ERC-20 token.
- However, if a bot that immediately takes the Ether sent at this time is running, the Ether will be stolen when the Ether is simply sent.
- We can use [Flashbots](https://docs.flashbots.net/) bundled transactions or just `permit` and `transferFrom` if the token is [EIP-2612 permit](https://eips.ethereum.org/EIPS/eip-2612) friendly.

| Challenge                                                                 | Note, Keywords |
| ------------------------------------------------------------------------- | -------------- |
| [EthernautDAO: 5. EthernautDaoToken](src/EthernautDAO/EthernautDaoToken/) |                |

### Claimable intermediate nodes of a Merkle tree
| Challenge                                             | Note, Keywords |
| ----------------------------------------------------- | -------------- |
| [Paradigm CTF 2022: MERKLEDROP](src/ParadigmCTF2022/) |                |

### Precompiled contracts
| Challenge                                         | Note, Keywords |
| ------------------------------------------------- | -------------- |
| [Paradigm CTF 2022: VANITY](src/ParadigmCTF2022/) |                |

### Faking errors
| Challenge                                       | Note, Keywords |
| ----------------------------------------------- | -------------- |
| [Ethernaut: 27. Good Samaritan](src/Ethernaut/) |                |

### Foundry cheatcodes
| Challenge                                            | Note, Keywords |
| ---------------------------------------------------- | -------------- |
| [Paradigm CTF 2022: TRAPDOOOR](src/ParadigmCTF2022/) |                |
| Paradigm CTF 2022: TRAPDOOOOR                        |                |

### Front-running
| Challenge                                               | Note, Keywords |
| ------------------------------------------------------- | -------------- |
| [DownUnderCTF 2022: Private Log](src/DownUnderCTF2022/) |                |

### Head overflow bug in calldata tuple ABI-reencoding (< Solidity 0.8.16)
- See: https://blog.soliditylang.org/2022/08/08/calldata-tuple-reencoding-head-overflow-bug/

| Challenge                                                 | Note, Keywords |
| --------------------------------------------------------- | -------------- |
| [0CTF 2022: TCTF NFT Market](src/0CTF2022/TctfNftMarket/) |                |

### Arbitrary storage overwriting by setting an array length to `2^256-1` (< Solidity 0.6.0)
- For example, any storage can be overwritten by negatively arithmetic overflowing the length of an array to `2^256-1`.
- It need not be due to overflow.
- The `length` property has been read-only since v0.6.0.

| Challenge                                          | Note, Keywords |
| -------------------------------------------------- | -------------- |
| [Capture The Ether: Mapping](src/CaptureTheEther/) |                |
| [Ethernaut: 19. Alien Codex](src/Ethernaut/)       |                |
| Paradigm CTF 2021: Bank                            |                |

### Constructor that is just a function by a typo (< Solidity 0.5.0)
- In versions before v0.4.22, the constructor is defined as a function with the same name as the contract, so a typo of the constructor name could cause it to become just a function, resulting in a bug.
- Since v0.5.0, this specification is removed and the `constructor` keyword must be used.

| Challenge                                                   | Note, Keywords |
| ----------------------------------------------------------- | -------------- |
| [Capture The Ether: Assume ownership](src/CaptureTheEther/) |                |
| [Ethernaut: 2. Fallout](src/Ethernaut/)                     |                |

### Storage overwrite via uninitialized storage pointer (< Solidity 0.5.0)
- Since v0.5.0, uninitialized storage variables are forbidden, so this bug cannot occur.

| Challenge                      | Note, Keywords                                                                      |
| ------------------------------ | ----------------------------------------------------------------------------------- |
| Capture The Ether: Donation    |                                                                                     |
| Capture The Ether: Fifty years |                                                                                     |
| ~~Ethernaut: Locked~~          | [deleted](https://forum.openzeppelin.com/t/ethernaut-locked-with-solidity-0-5/1115) |

### Other ad-hoc vulnerabilities and methods
| Challenge                                                         | Note, Keywords                                                                                                                    |
| ----------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------- |
| [Paradigm CTF 2021: Bouncer](src/ParadigmCTF2021/Bouncer/)        | The funds required for batch processing are the same as for single processing.                                                    |
| Paradigm CTF 2021: Market                                         | Make the value of one field be recognized as the value of another field by using key misalignment in the Eternal Storage pattern. |
| [EthernautDAO: 2. WalletLibrary](src/EthernautDAO/WalletLibrary/) | m and n of m-of-n multisig wallet can be changed.                                                                                 |
| [Paradigm CTF 2022: RESCUE](src/ParadigmCTF2022/)                 |                                                                                                                                   |
| Paradigm CTF 2022: JUST-IN-TIME                                   |                                                                                                                                   |
| Paradigm CTF 2022: 0XMONACO                                       |                                                                                                                                   |
| [BalsnCTF 2022](src/BalsnCTF2022/)                                | initialize, `_safeTransferFrom`, `CREATE2`                                                                                        |

## Bitcoin
Note
- Including challenges of Bitcoin variants whose transaction model is Unspent Transaction Output (UTXO).

### Bitcoin basics
| Challenge                              | Note, Keywords              |
| -------------------------------------- | --------------------------- |
| TsukuCTF 2021: genesis                 | genesis block               |
| WORMCON 0x01: What's My Wallet Address | Bitcoin address, RIPEMD-160 |

### Recovery of a private key by the same nonce attack
- There was a bug and it has been fixed using [RFC6979](https://datatracker.ietf.org/doc/html/rfc6979).
- https://github.com/daedalus/bitcoin-recover-privkey

| Challenge                                 | Note, Keywords |
| ----------------------------------------- | -------------- |
| [darkCTF: Duplicacy Within](src/DarkCTF/) |                |

### Bypassing PoW of other applications using Bitcoin's PoW database
- Bitcoin uses a series of leading zeros in the SHA-256 hash value as a Proof of Work (PoW), but if other applications are designed in the same way, its PoW time can be significantly reduced by choosing one that matches the conditions from Bitcoin's past PoW results 

| Challenge                   | Note, Keywords |
| --------------------------- | -------------- |
| Dragon CTF 2020: Bit Flip 2 | 64-bit PoW     |

## Cairo
| Challenge                                                       | Note, Keywords   |
| --------------------------------------------------------------- | ---------------- |
| [Paradigm CTF 2022: RIDDLE-OF-THE-SPHINX](src/ParadigmCTF2022/) | contract call    |
| [Paradigm CTF 2022: CAIRO-PROXY](src/ParadigmCTF2022/)          | integer overflow |
| [Paradigm CTF 2022: CAIRO-AUCTION](src/ParadigmCTF2022/)        | `Uint256`        |
| [BalsnCTF 2022: Cairo Reverse](src/BalsnCTF2022/)               | reversing        |

## Solana
| Challenge                                             | Note, Keywords       |
| ----------------------------------------------------- | -------------------- |
| ALLES! CTF 2021: Secret Store                         | `solana`,`spl-token` |
| ALLES! CTF 2021: Legit Bank                           |                      |
| ALLES! CTF 2021: Bugchain                             |                      |
| ALLES! CTF 2021: eBPF                                 | Reversing eBPF       |
| [Paradigm CTF 2022: OTTERWORLD](src/ParadigmCTF2022/) |                      |
| [Paradigm CTF 2022: OTTERSWAP](src/ParadigmCTF2022/)  |                      |
| Paradigm CTF 2022: POOL                               |                      |
| Paradigm CTF 2022: SOLHANA-1                          |                      |
| Paradigm CTF 2022: SOLHANA-2                          |                      |
| Paradigm CTF 2022: SOLHANA-3                          |                      |

## Other blockchain-related
- Something that is not a blockchain but is part of the ecosystem.

### IPFS
- InterPlanetary File System (IPFS)

| Challenge                              | Note, Keywords                 |
| -------------------------------------- | ------------------------------ |
| TsukuCTF 2021: InterPlanetary Protocol | Address is Base32 in lowercase |
