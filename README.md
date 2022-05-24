# cadence-quests
Flow Cadence Quests w/ Emerald City

## Quest 1 

1. Explain what a blockchain is

Blockchains are a sequence of verifiable "blocks" (I.E. groups) of transactions, maintained by a network of peer nodes.  These nodes implement a common verification system to ensure that each is behaving honestly.  For example, Bitcoin repurposed Adam Back's Hashcash as a proof of work mechanism such that "miner" nodes are only eligible to mint the next block if they discover a computationally expensive pattern within a hash of that block.  (They can search for this treasure hunt by adjusting a nonce value as an input to the hashing function.)  Blockchains such as Flow use a proof of stake verification system, where nodes are trusted based upon their allocations of native tokens (FLOW in this case) rather than the computational/power resources of proof of work.

2. Explain what a smart contract is

A smart contract is a program that can be deployed to a blockchain to interact with other participants on the blockchain as specified by whatever logic it is encoded with.  Typically a smart contract will have its own wallet address that can both receive and send native tokens and NFTs.  Each blockchain will have its own languages for writing smart contracts; for Flow that language is called Cadence.

3. What is the difference between a script and a transaction?

In the context of the Flow-ecosystem, a transaction is an exchange of tokens or metadata written into the blockchain.  Such transactions are commonly executed by a function call within a smart contract that costs native resouces to execute.  Scripts meanwhile (in Flow parlance) are able to read data from the blockchain without mutating blockchain state.

## Quest 2

1. What are the 5 Cadance Pillars?

Safety/Security, Clarity, Approchability, Developer Experience, Resource Oriented Programming

One could argue that Clarity, Approachability, and Developer Experience could be consolidated into a single pillar.

2. Why are the Pillars useful?

Provide a reliable platform for dapp development that's easy to use.

# Quest Ch2.1

Account 0x03
```
pub contract JacobTucker {
    pub let is: String

    init() {
        self.is = "the best"
    }
}
```

Script Template
```
import JacobTucker from 0x03

pub fun main(): String {
  return JacobTucker.is
}
```
