# NFT-Tutorial-Using-Hardhat

# Introduction

In this tutorial we will learn how to use HardHat which is a popular framework used for smart contract development. It is a JavaScript based framework like Truffle but has certain advantages over Truffle which we will go over in the tutorial. The goal of this tutorial is to build an awesome NFT project where we can convert anything into an NFT. We will also learn how to write, test and deploy smart contracts using the Hardhat framework.

# Prerequisites

This tutorial assumes you have a basic understanding of what smart contracts are & how they are used for building dApp projects. Ideally, you also know about and have used Metamask and are familiar with the concept of public and private keys. Familiarity with Solidity and Javascript along with Node.js will also be very helpful.

# Requirements

Please make sure you have the following software installed:

- [Node.js](https://nodejs.org/en/) v14.17.6 LTS or higher, for installing HardHat and other node packages.
- [MetaMask](https://metamask.io/) for interacting with the blockchain. 
  - Once you have installed MetaMask, add a connection with the [Polygon Mumbai testnet](https://docs.matic.network/docs/develop/metamask/config-polygon-on-metamask).
- Remember to get your account funded with testnet MATIC tokens using the [Polygon faucet](https://faucet.polygon.technology/).

# Creating Polygonscan API key

When we deploy our contract to the blockchain (either mainnet or testnet), it is a good practice to verify the code of our smart contract after deploying it. If our smart contract is **verified**, then the smart contract code will be visible on the block explorer and users will be able to interact with the smart contract directly from the block explorer (such as Polygonscan). Verifying the source code is highly encouraged as it makes our project more transparent, and users are more likely to interact with it.

Using a HardHat plugin, smart contracts can be verified automatically during the deployment process. To do this, we will need a Polygonscan API key. Follow these steps to get your own API key:

1. Open [Polygonscan](https://polygonscan.com/).
2. Click on **SignIn** in the upper right corner of the page.
3. If you already have an account, enter your username and password to login, or else create your new account by visiting [https://polygonscan.com/register](https://polygonscan.com/register).
4. Once you are logged in, go to the API-KEYs section on the left sidebar.
5. Click on the "Add" button, give it a name and click on continue.

You now have an API key which will allow you to access the Polygonscan API features such as contract verification. This key will be same for both mainnet and testnet.

# Creating Hardhat project

To install HardHat, run the command:

```bash
npm install -g hardhat
```

This will install HardHat globally so that later we can make use of the `npx` command to create HardHat projects. 

Now to create our project, we will use the following code

```bash
mkdir art_gallery # I am naming my project folder as art_gallery but any other name works
cd art_gallery    # mode into the folder
npx hardhat
```

On typing the last command, something similar to this should appear on your screen:

![Hardhat Initializing](../../../.gitbook/assets/HardHatInitializing.png)

We can either start with a basic sample project so that it is easier for us to understand, so let's hit enter. After this we will be asked to set our project root. Hit Enter to keep the default value. Next it will ask if we want a .gitignore file. Hit enter to keep the default value of yes or type n for no. It will then ask if we want to install the dependencies for our sample project. Hit enter to keep the default of yes. Now it is going to create a sample project for us and install the dependencies. Once done it will look something like this:

![Hardhat Initialized](../../../.gitbook/assets/HardhatInstalled.png)

**Congrats** 🎊🎊🎊 you have created your first Hardhat project.

# Understanding the code

Now let's open our project and take a cursory look at what's in it. I will be using VSCode as my editor of choice but feel free to use any other editor of choice.

![Initial File Structure](../../../.gitbook/assets/InitialFileStructure.png)

What we get is a very simple project. The folder names are quite self explanatory. All our smart contracts, script files and test scripts are going to be in their respective folders. 

`hardhat.config.js` is the file which contains all the configurations specific to hardhat.

Before we start writing our smart contract, let's look at the `hardhat.config.js` file which is the heart of our HardHat project. The contents of this file by default are:

```jsx
require("@nomiclabs/hardhat-waffle");

// This is a sample Hardhat task. To learn how to create your own go to
// https://hardhat.org/guides/create-task.html
task("accounts", "Prints the list of accounts", async (taskArgs, hre) => {
  const accounts = await hre.ethers.getSigners();

  for (const account of accounts) {
    console.log(account.address);
  }
});

// You need to export an object to set up your config
// Go to https://hardhat.org/config/ to learn more

/**
 * @type import('hardhat/config').HardhatUserConfig
 */
module.exports = {
  solidity: "0.6.6",
};
```

We start by importing the `@nomiclabs/hardhat-waffle` package which will give us access to the `hre` class. HRE is short for **Hardhat Runtime Environment** which is an object that contains all the functionality that Hardhat exposes. You can think of it as "Hardhat *is* hre".

Next is where we define various tasks. This tasks can be ran by typing `npx hardhat <TASK_NAME>`

While developing our project we are also doing to write our own custom tasks.

In the end is `module.export` , this is where we are going to list various parameters like compiler version, networks to use, API keys, etc. Please note, here we have defined the solidity version as `0.8.4` .

# Writing the Smart Contract

In this section we are going to write our first smart contract which will be an NFT Creation Smart contract. The smart contract will ask for a tokenURI and will convert it into an NFT. We will also learn about how to install various libraries and how to use them in our smart contract.

## Installing Openzeppelin library

While writing any program, we always prefer using various libraries so that we don't have to write everything from scratch. Since we are going to build an NFT based project we are going to follow the standards defined in EIP721. The best way to do so, is to import the ERC721 contract present in Openzeppelin/contracts library and only making the necessary changes. To install the package, open terminal and type

```bash
npm install @openzeppelin/contracts
```

## Starting our smart contract

Let's create a new file names `Artwork.sol` inside the contracts directory. This is going to be our first smart contract which will help us in creating NFTs.

 

```jsx
//SPDX-License-Identifier: Unlicense
pragma solidity 0.8.4; 

contract Artwork {}
```

We start by defining the License of our smart contract. For this tutorial we are keeping it unlicensed. If we don't define the license, it is going to give an error during the compile time. `pragma` keyword is used to define the solidity version we are going to use. Make sure you are using the same solidity version as defined in the `hardhat.config.js` file.

## Importing contract

Next we are going to import the ERC721 smart contract from openzeppelin library which we just installed. After the line defining the solidity version and before defining the contract, import the ERC721 contract

```jsx
import "@openzeppelin/contracts/token/ERC721/ERC721.sol";
```

## Inheriting ERC721 and constructor initialisation

Make the following modifications to the code:

```jsx
//SPDX-License-Identifier: Unlicense
pragma solidity 0.8.4; 

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract Artwork is ERC721 {

    constructor(
        string memory name,
        string memory symbol
    ) ERC721(name, symbol) {}

}
```

Here we are doing the following things:

- Inheriting the ERC721 smart contract into our Artwork smart contract using the `is` keyword.
- Constructor is always the first function that is called while deploying a smart contract. Since we are inheriting another smart contract, we have to pass in the values for the constructor of that smart contract while defining our constructor. Here we taking name and symbol as constructor arguments and are passing them to the constructor of ERC721.
- `name` and `symbol` are going to be the name and symbol of our NFT respectively.

## Defining tokenCounter

NFTs are called Non-Fungible because all of them are unique. What makes them unique is the token id assigned to them. We are going to define a global variable called tokenCounter and use it for calculating the token id. It will start with zero and increment by 1 for every new NFT that is minted. The value of tokenCounter is set to 0 in the constructor.

```jsx
//SPDX-License-Identifier: Unlicense
pragma solidity 0.8.4; 

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract Artwork is ERC721 {

    uint256 public tokenCounter;

    constructor(
        string memory name,
        string memory symbol
    ) ERC721(name, symbol) {
        tokenCounter = 0;
    }

}
```

## Creating the mint function

Now we are going to define a mint function which can be called by any user in order to mint new NFTs. Each NFT have certain data associated with it. In our case we are trying to convert images or other collectibles into NFT and hence the image should be somehow stored in the smart contract. Since storing data on blockchain costs gas, it won't be financially feasible if the entire image and other associated data is stored. So, we host the image separately along with a JSON file which contains all the details about the NFT. The image and JSON file can be hosted separately either decentralised (using IPFS) or centrally using traditional methods. The JSON file contains the link to the image as well. Once the JSON file is hosted, the link pointing to that JSON file is stored in the blockchain in the as tokenURI. tokenURI roughly translates to "token Universal Resource Identifier". [This](https://opensea-creatures-api.herokuapp.com/api/creature/2) is an example of centrally hosted tokenURI. The mint function will be:

```jsx
function mint(string memory _tokenURI) public {
        _safeMint(msg.sender, tokenCounter);
        _setTokenURI(tokenCounter, _tokenURI);

				tokenCounter++;
    }
```

_safeMint is another function present in the ERC721 contract which is used to mint new NFTs. It takes to parameters:

- to: This is the first parameter and this takes address of account which will own the NFT after it is minted.
- tokenId: This will be the tokenId of the new minted NFT.

`msg.sender` is a special keyword which returns the account calling the smart contract. In this this case it will return the account calling the mint function. Hence the account calling the mint function will be passed as first argument and therefore the minted NFT will be owned by this account. 

The `_setTokenURI()` is not yet defined so please ignore it for the moment. This function will be used for setting the tokenURI for the minted NFT. This function was present in the ERC721 library but have been discontinued after version 0.8.0 and hence we will be writing it ourselves.

Once the token is minted and tokenURI is set we increment the tokenCounter by 1 so that the next minted token have a new token id.

## Creating the _setTokenURI() function

Our NFT smart contract must store all the valid token id with their respective tokenURI. For this we can use the `mapping` datatype in solidity. In solidity mapping is just like how hashmap works in other programming languages like Java. We can define a mapping from a `uint256` number to a `string` which will signify that each token id is *mapped* to its respective tokenURI. Just after the declaration of tokenCounter variable, define the mapping.

```jsx
mapping (uint256 => string) private _tokenURIs;
```

Now let's write the _setTokenURI function:

```jsx
function _setTokenURI(uint256 _tokenId, string memory _tokenURI) internal virtual {
        require(
            _exists(_tokenId),
            "ERC721Metadata: URI set of nonexistent token"
        );  // Checks if the tokenId exists
        _tokenURIs[_tokenId] = _tokenURI;
    }
```

There are many new terms defined here so let's deal with them one-by-one:

- **internal:** The function is defined with internal keyword. It means this function can be called only by other functions in this smart contract or other smart contracts inheriting this smart contract. This function `CAN'T` be called by an external user.
- **virtual:** This keyword means that this function can be override by any contract that is inheriting this smart contract
- **require:** The first thing inside the function body is the `require` keyword. It takes in a conditional statement. If this statement returns `true` then only rest of the smart contract is executed. If the conditional statement returns `false` , then it will generate an error. The second parameter is the generated error message and it is optional.
- **_exists():** This function returns true if there is a token minted with the passed tokenId else returns `false`.

Therefore, this function first makes sure that the tokenId for which we are trying to set the tokenURI is already minted and if so, it will add the tokenURI to the mapping along with the respective tokenId.

## Creating tokenURI() function

The last function which we have to create is called the tokenURI() function. It will be a publicly callable function which will take tokenId as a parameter and return its respective tokenURI. This is a standard function which is called by various NFT based platforms like [OpenSea](https://opensea.io/). Platforms like this use the tokenURI returned from this function to display various information about the NFT like its properties and the display image. 

Let's write the tokenURI function:

```jsx
function tokenURI(uint256 _tokenId) public view virtual override returns(string memory) {
        require(
            _exists(_tokenId),
            "ERC721Metadata: URI set of nonexistent token"
        );
        return _tokenURIs[_tokenId];
    }
```

- **public:** This function is public which means any outside user can call it.
- **view:** Since this function doesn't change the state of the blockchain, i.e. it doesn't change any value in the smart contract, executing this function will not require any Gas. Since no state change will take place, this function is defined as `view` .
- **override:** We already have a tokenURI() function in the ERC721 contract we have inherited which uses the concept of `baseURI + tokenId` to return the tokenURI. Since we need a different logic hence we had to "over wright" the inherited function by using this keyword.
- **returns(string memory):** Since this function returns a string value we have to define it while declaring the function.

This function first checks whether the tokenId passed was actually minted, and if the token was minted, it returns the tokenURI from the mapping.

## Putting it all together

Putting all the functions together, the final smart contract will look like:

```jsx
//SPDX-License-Identifier: Unlicense
pragma solidity 0.8.4; 

import "@openzeppelin/contracts/token/ERC721/ERC721.sol";

contract Artwork is ERC721 {

    uint256 public tokenCounter;
    mapping (uint256 => string) private _tokenURIs;

    constructor(
        string memory name,
        string memory symbol
    ) ERC721(name, symbol) {
        tokenCounter = 0;
    }

    function mint(string memory _tokenURI) public {
        _safeMint(msg.sender, tokenCounter);
        _setTokenURI(tokenCounter, _tokenURI);

        tokenCounter++;
    }

    function _setTokenURI(uint256 _tokenId, string memory _tokenURI) internal virtual {
        require(
            _exists(_tokenId),
            "ERC721Metadata: URI set of nonexistent token"
        );  // Checks if the tokenId exists
        _tokenURIs[_tokenId] = _tokenURI;
    }

    function tokenURI(uint256 _tokenId) public view virtual override returns(string memory) {
        require(
            _exists(_tokenId),
            "ERC721Metadata: URI set of nonexistent token"
        );
        return _tokenURIs[_tokenId];
    }

}
```

# Compiling the Smart Contract

Now our smart contract is ready, we are have to compile it. In order to compile your smart contract type:

```bash
npx hardhat compile
```

If everything went as expected you will be getting the message `Compilation finished successfully` . If the contract doesn't compile successfully or there are error, try to read the tutorial again to find out where it went wrong. Some of the possible mistakes are:

- The `SPDX-License-Identifier` is not provided
- There is a mismatch between the solidity compiler version defined using `pragma` keyword and the version defined in `hardhat.config.js` file.
- There is a version mismatch between the imported smart contracts and the version used to write the smart contract. To solve this, double check the version of openzeppelin you are installing with npm. In my case, the npm package have version `4.3.2` and the smart contracts are written with solidity version `0.8.0`.

# Writing tests

Til now we have written our smart contract and have compiled it. A successfully compiled smart contract doesn't mean that it is correct. It is very important to write test cases to ensure that it passes all the intended situations and some edge cases. Testing smart contracts become even more important since once a smart contract is deployed to the blockchain it cannot be updated later.

We are going to use `chai` library for writing our tests. This library should have been already installed while creating our project, if not you can type `npm install --save-dev chai` .

We are going to test our smart contract for the following:

- NFT is minted successfully: After an account calls the mint function, an NFT is minted for that account and it's balance increases.
- tokenURI is set successfully: For two tokens minted with different tokenURI, both tokens have their respective tokenURI and the data can be retrieved properly.

## Getting started with  writing tests

In the test folder there will be a script called sample-test.js. We will be delete this and create a new file called `artwork-test.js` in the same `test` directory. You can use any other name to your choice as well. In this file paste in the following code:

```jsx
const { expect } = require('chai');
const { ethers } = require("hardhat")

describe("Artwork Smart Contract Tests", function() {

    this.beforeEach(async function() {
        // This is executed before each test
    })

    it("NFT is minted successfully", async function() {

    })

    it("tokenURI is set sucessfully", async function() {

    })

})
```

- **describe:** This keyword is used to give a name to the set of tests we are going to perform.
- **beforeEach:** The function defined inside this will be executed before every test case. We will be deploying the NFT contract here because the contract must be deployed before each test is ran.
- **it:** This is used to write each test case. It takes in a title for the test and a function which runs the test case.

**NOTE: Unlike truffle, in hardhat there is no need to run `ganache-cli` separately for tests. Hardhat have it's own local testnet which we can use.**

## Deploying contract and writing tests

In order to deploy the smart contract we have to first get a reference to the compiled smart contract using `ethers.getContractFactory()` and then we can use the `deploy()` method to deploy the smart contract and pass in the arguments. We do this in the `beforeEach()` section.

```jsx
let artwork;

this.beforeEach(async function() {
        // This is executed before each test
        // Deploying the smart contract
        const Artwork = await ethers.getContractFactory("Artwork");
        artwork = await Artwork.deploy("Artwork Contract", "ART");
})
```

For checking whether the NFT is properly minted, we first get one of the accounts created by hardhat. Then we call the mint function present in the smart contract with a random tokenURI. We check the balance of the account before and after minting and it should be 0 and 1 respectively. If it passes the test, it means that NFTs are minted properly.

```jsx
it("NFT is minted successfully", async function() {
        [account1] = await ethers.getSigners();

        expect(await artwork.balanceOf(account1.address)).to.equal(0);
        
        const tokenURI = "https://opensea-creatures-api.herokuapp.com/api/creature/1"
        const tx = await artwork.connect(account1).mint(tokenURI);

        expect(await artwork.balanceOf(account1.address)).to.equal(1);
    })
```

For checking whether tokenURI are set properly, we take two random tokenURI and set them from different accounts. Then we call the tokenURI() function to get the tokenURI for respective token and match them with the passed argument to ensure that the tokenURI are set correctly.

```jsx
it("tokenURI is set sucessfully", async function() {
        [account1, account2] = await ethers.getSigners();

        const tokenURI_1 = "https://opensea-creatures-api.herokuapp.com/api/creature/1"
        const tokenURI_2 = "https://opensea-creatures-api.herokuapp.com/api/creature/2"

        const tx1 = await artwork.connect(account1).mint(tokenURI_1);
        const tx2 = await artwork.connect(account2).mint(tokenURI_2);

        expect(await artwork.tokenURI(0)).to.equal(tokenURI_1);
        expect(await artwork.tokenURI(1)).to.equal(tokenURI_2);

    })
```

## Putting it all together

So finally, after putting all the test cases together, the content of `artwork-test.js` file will be:

```jsx
const { expect } = require('chai');
const { ethers } = require("hardhat")

describe("Artwork Smart Contract Tests", function() {

    let artwork;

    this.beforeEach(async function() {
        // This is executed before each test
        // Deploying the smart contract
        const Artwork = await ethers.getContractFactory("Artwork");
        artwork = await Artwork.deploy("Artwork Contract", "ART");
    })

    it("NFT is minted successfully", async function() {
        [account1] = await ethers.getSigners();

        expect(await artwork.balanceOf(account1.address)).to.equal(0);
        
        const tokenURI = "https://opensea-creatures-api.herokuapp.com/api/creature/1"
        const tx = await artwork.connect(account1).mint(tokenURI);

        expect(await artwork.balanceOf(account1.address)).to.equal(1);
    })

    it("tokenURI is set sucessfully", async function() {
        [account1, account2] = await ethers.getSigners();

        const tokenURI_1 = "https://opensea-creatures-api.herokuapp.com/api/creature/1"
        const tokenURI_2 = "https://opensea-creatures-api.herokuapp.com/api/creature/2"

        const tx1 = await artwork.connect(account1).mint(tokenURI_1);
        const tx2 = await artwork.connect(account2).mint(tokenURI_2);

        expect(await artwork.tokenURI(0)).to.equal(tokenURI_1);
        expect(await artwork.tokenURI(1)).to.equal(tokenURI_2);

    })

})
```

You can run the test by typing the following command in terminal:

```bash
npx hardhat test
```

and the output will be something like:

![Running Tests](../../../.gitbook/assets/ranTest.png)

# Deploy the smart contract

By now we have learned how to write smart contracts and test them. Now its time we finally move towards deploying our smart contract to Polygon Testnet and showoff our new learned skills to our friends 😎😎😎.

## Installing some more packages

Before we move ahead and deploy our smart contract, we will need two additional npm packages:

- **dotenv:** Type `npm install dotenv` . This will be used to manage environment variables. We will use environment variable for setting up our API key.
- **@nomiclabs/hardhat-etherscan:** Type `npm install @nomiclabs/hardhat-etherscan`. This library is used to verify the smart contract while deploying it to the network. The process of verifying smart contract in `etherscan` and `polygonscan` are exactly similar.

## Setting up environment variable

![Creating ENV File](../../../.gitbook/assets/WithENV.png)

Create a new file named `.env` in the project root directory

Create a new entry in the .env file, `POLYGONSCAN_KEY` and set it to the API key created in the beginning of the tutorial and another entry `PRIVATE_KEY` and set it to the private key of the testnet account with MATIC in it.

```bash
POLYGONSCAN_KEY=axaxaxaxaxaxaxaxaxaxaxaxaxaxaxaxaxax
PRIVATE_KEY=axaxaxaxaxaxaxaxaxaxaxaxaxaxaxaxaxa
```

## Modifying the config file

In order to deployed a verified smart contract to Mumbai testnet, we have to make certain changes in the `hardhat.config.js` file. First copy paste this entire code into the file and then we will understand it step by step

```bash
require("@nomiclabs/hardhat-waffle");
require("@nomiclabs/hardhat-etherscan")
require("dotenv").config();

task("accounts", "Prints the list of accounts", async (taskArgs, hre) => {
  const accounts = await hre.ethers.getSigners();

  for (const account of accounts) {
    console.log(account.address);
  }
});

task("deploy", "Deploy the smart contracts", async(taskArgs, hre) => {

  const Artwork = await hre.ethers.getContractFactory("Artwork");
  const artwork = await Artwork.deploy("Artwork Contract", "ART");

  await artwork.deployed();

  await hre.run("verify:verify", {
    address: artwork.address,
    constructorArguments: [
      "Artwork Contract",
      "ART"
    ]
  })

})

module.exports = {
  solidity: "0.8.4",
  networks: {
    mumbai: {
      url: "https://matic-testnet-archive-rpc.bwarelabs.com",
      accounts: [
        process.env.PRIVATE_KEY,
      ]
    }
  },
  etherscan: {
    apiKey: process.env.POLYGONSCAN_KEY,
  }
};
```

Let's now understand what we are doing here:

- We have imported the `@nomiclabs/hardhat-etherscan` package which is a hardhat plugin that enables us to verify our smart contract after we have deployed it to the network.
- For verifying the smart contract, we will need the API Key we have created towards the start of the tutorial. That API key is passed with the `module.export` . Please note that the object name is `etherscan` , and the term `apiKey` is used to refer to the key we created.
- We also have created a new task for deploying our smart contract. To create the task, we use `task` keyword and pass it a name and description. In this task we have written the code for deploying our smart contract. After deploying the contract, we call the `verify:verify` task, made available to us by hardhat-etherscan plugin. We passed the contract address along with the parameters passed to the constructor while deploying the contract.
- Finally since we are going to deploy our contract to mumbai testnet, we created a new entry in the network section and added the RPC url to Mumbai Testnet along with the private key of the account which will be used to deploy the smart contract.

## The moment of truth

Now finally we are ready to deploy our contract to testnet. Type in the following command:

```bash
npx hardhat deploy --network mumbai
```

Now we have to wait for few minutes as the contract is deployed and verified. 🤞🤞🤞

If everything is correct you should get an output similar to this:

![Deploying Contract](../../../.gitbook/assets/DeployingContract.png)

You can now checkout your contract at the link provided in the output. You can checkout my deployed contract [here](https://mumbai.polygonscan.com/address/0x12Fc3C44b4092aD55cf0212fa3A84a1210fCED5f).

## Common Errors while deploying

### Insufficient Fund

If the private key you passed belongs to an account which don't have sufficient token to deploy the contract, you will get this error:

```bash
An unexpected error occurred:

Error: insufficient funds for intrinsic transaction cost (error={"name":"ProviderError","code":-32000,"_isProviderError":true}, method="sendTransaction", transaction=undefined, code=INSUFFICIENT_FUNDS, version=providers/5.4.5)
    at Logger.makeError (/Volumes/Samsung_T5/Work/eth-project/tutorials/figment-polygon-tutorials/art_gallery_take1/art_gallery/node_modules/@ethersproject/logger/src.ts/index.ts:225:28)
    at Logger.throwError (/Volumes/Samsung_T5/Work/eth-project/tutorials/figment-polygon-tutorials/art_gallery_take1/art_gallery/node_modules/@ethersproject/logger/src.ts/index.ts:237:20)
    at checkError (/Volumes/Samsung_T5/Work/eth-project/tutorials/figment-polygon-tutorials/art_gallery_take1/art_gallery/node_modules/@ethersproject/providers/src.ts/json-rpc-provider.ts:53:16)
    at /Volumes/Samsung_T5/Work/eth-project/tutorials/figment-polygon-tutorials/art_gallery_take1/art_gallery/node_modules/@ethersproject/providers/src.ts/json-rpc-provider.ts:215:24
    at processTicksAndRejections (node:internal/process/task_queues:96:5) {
  reason: 'insufficient funds for intrinsic transaction cost',
  code: 'INSUFFICIENT_FUNDS',
  error: ProviderError: insufficient funds for gas * price + value
      at HttpProvider.request (/Volumes/Samsung_T5/Work/eth-project/tutorials/figment-polygon-tutorials/art_gallery_take1/art_gallery/node_modules/hardhat/src/internal/core/providers/http.ts:49:19)
      at LocalAccountsProvider.request (/Volumes/Samsung_T5/Work/eth-project/tutorials/figment-polygon-tutorials/art_gallery_take1/art_gallery/node_modules/hardhat/src/internal/core/providers/accounts.ts:162:36)
      at processTicksAndRejections (node:internal/process/task_queues:96:5)
      at EthersProviderWrapper.send (/Volumes/Samsung_T5/Work/eth-project/tutorials/figment-polygon-tutorials/art_gallery_take1/art_gallery/node_modules/@nomiclabs/hardhat-ethers/src/internal/ethers-provider-wrapper.ts:13:20),
  method: 'sendTransaction',
  transaction: undefined
```

In this case, please fund your account using the [faucet](https://faucet.polygon.technology/).

### Invalid API Key

If the Polygonscan API Key you entered is missing or is not valid, you are going to get the following error message:

```bash
Nothing to compile
Compiling 1 file with 0.8.4
Error in plugin @nomiclabs/hardhat-etherscan: Invalid API Key

For more info run Hardhat with --show-stack-traces
```

In this case please make sure you are entering the correct API key.

# Finally trying our smart contract

If we visit our smart contract you can see that our contract is verified, that is denoted by a Green Tick Sign in the contracts tab. In the Contracts tab, click on `Write Contract` . Now click on `Connect to Web3` and connect your MetaMask account.

![Contract Deployed and Verified](../../../.gitbook/assets/ContractVerified.png)

Now select the `mint` action and put your tokenURI. I am using [this](https://gambakitties-metadata.herokuapp.com/metadata/1) as my tokenURI. If you want you can create your own tokenURI by hosting it using IPFS. Once you put your tokenURI, click on the `write` button. You will get a MetaMask popup. Confirm the transaction and wait for it to be successful. 

![MetaMask Sign Popup](../../../.gitbook/assets/MetaMaskSign.png)

WALAAAA!! 🥳🥳🥳, Your NFT is successfully minted. You can visit [Opensea Testnet](https://testnets.opensea.io/) and in the my profile section you can now view your NFT. Checkout the one we created [here](https://testnets.opensea.io/assets/mumbai/0x12fc3c44b4092ad55cf0212fa3a84a1210fced5f/0).

![NFT in Opensea Testnet](../../../.gitbook/assets/NFTMinted.png)

# Conclusion

So in this tutorial we learned some of the basics of Hardhat. We wrote a smart contract that helps us create NFTs, wrote tests for our smart contract and finally deployed them to the Mumbai testnet and also verified our contract. Using a similar procedure, we can build any defi project of our choice and deploy them to any network of our liking (Ethereum, Polygon, Binance Smart Chain, AVAX etc.). 

# Next Step

We are not going to stop right here, moving forward we will also learn how to build a NFT-Marketplace contract, where various NFTs can be listed, bought and sold. We will also look into some more aspects of NFT and learn new things. 😎😎😎

# About The Author

Hi, my name is Bhaskar Dutta and I am a blockchain Developer, Researcher and Freelancer. I am always looking forward to learn new things and discuss about Sitcoms. To know me better, you can checkout my [Github](https://github.com/BhaskarDutta2209).

# Reference

I found the following very helpful while learning:

- [Hardhat Official Documentation](https://hardhat.org/)
- [Matic Official Documentation](https://docs.matic.network/docs/develop/getting-started)
- [OpenZeppelin Forun Page](https://forum.openzeppelin.com/t/function-settokenuri-in-erc721-is-gone-with-pragma-0-8-0/5978/3)