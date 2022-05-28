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

## Quest 2.1

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

## Quest 2.2

1. changeGreeting mutates a value so would not be appropriate in a script, which is read-only
2. AuthAccount references an account address on whose behalf the transaction will take place
3. the body of `prepare` is for non-mutating set-up work while `execute` is for isolating blockchain state changes in a separate function
4. code below

0x01 Account
```
pub contract HelloWorld {

    pub var myNumber: Int

    init() {
        self.myNumber = 0
    }

    pub fun updateNumber(newNumber: Int) {
        self.myNumber = newNumber
    }

}
```

Script
```
import HelloWorld from 0x01

pub fun main(): Int {
  return HelloWorld.myNumber
}
```

Transaction
```
import HelloWorld from 0x01

transaction(myNewNumber: Int) {

  prepare(signer: AuthAccount) {}

  execute {
    HelloWorld.updateNumber(newNumber: myNewNumber)
  }
}
```

## Quest 2.3
Initialize an array of your favorite 3 ppl and log them
```
pub fun main(): Void {
  let array: [String] = ["Me", "You", "Roham"]
  log(array)
}
```
Initialize a dictionary of UInt64:String w/ Social media Ratings
```
pub fun main(): Void {
  let myFavSocials: {UInt64: String} = {1: "YouTube", 2: "Instagram", 3: "Reddit", 4: "Twitter", 5: "LinkedIn", 6: "Facebook" }
  log(myFavSocials)
}
```

The force unwrap operator `!` will panic if the variable it follows is `nil`

The following code results in an error because it's syntactically possible for the dictionary `thing` to not have a value for address `0x03` which means we either need to add force unwrap `!` to the return or (preferably) change the function return type to `String?` to indicate a `nil` return value is possible:
```
pub fun main(): String {
  let thing: {Address: String} = {0x01: "One", 0x02: "Two", 0x03: "Three"}
  return thing[0x03]
}
```

## Quest 2.4
1) Create a struct within a contract:
```
pub contract Gallery {

  pub struct Image {
    pub let filename: String
    pub let width: UInt64
    pub let height: UInt64

    init(_filename: String, _width: UInt64, _height: UInt64) {
      self.filename = _filename
      self.width = _width
      self.height = _height
    }
  }
}
```

2) Create a dictionary or array that contains the Struct:

```
  pub var images: [Image]
  
  init () {
    self.images = []
  }

```

3) Create a function to add to that array/dictionary:

```
  pub fun addImage(filename: String, width: UInt64, height: UInt64) {
    self.images.append( Image(_filename: filename, _width: width, _height: height) )
  }
```

4) Add a transaction to call that function in step 3:

```
import Gallery from 0x02

transaction(filename: String, width: UInt64, height: UInt64) {

    prepare(signer: AuthAccount) {}

    execute {
        Gallery.addImage(filename: filename, width: width, height: height)
        log("We're done.")
    }
}
```

5) Add a script to read the Struct you defined
```
import Gallery from 0x02

pub fun main(account: Address): Gallery.Image {
    return Gallery.images[0]! // return first image from gallery
}
```

## Quest 3.1

1) Resources are different from structs in that they can only exist inside contracts, need to be explicitly created and destroyed, and cannot be copied.
2) A `resource` would be appropriate to use when we're looking to represent ownership over something to ensure we are explicit about custody
3) We can use the `create` keyword for a resource
4) A resource must be created in a contract, so cannot be created within a script or transaction
5) `pub resource Jacob {}` is declaring a type of `@Jacob` for the resource
6) Correcting the following code:

```
pub contract Test {

    // Hint: There's nothing wrong here ;)
    pub resource Jacob {
        pub let rocks: Bool
        init() {
            self.rocks = true
        }
    }

    pub fun createJacob(): @Jacob { // must use @ as prefix to resource return type
        let myJacob <- create Jacob() // use create keyword and <- symbol to assign to myJacob
        return <- myJacob // use <- symbol to return resource
    }
}
```
