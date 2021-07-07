# Startfeld's ERC-20 Smart Contract
This is Startfelds's [ERC-20](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20) smart contract, whose tokens are used as vouchers in their ecosystem.

## Changelog
See the created [`CHANGELOG`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/CHANGELOG.md) file in this repository.

## Installation
### 1. Install the Truffle Framework
We use the Truffle framework for the compilation, testing, and deployment. Please follow their [guide](https://www.trufflesuite.com/truffle) to install the framework on your computer.

### 2. Getting Started
Run `npm i` in order to install the necessary [OpenZeppelin node modules](https://www.npmjs.com/package/@openzeppelin/contracts) as well as further required dependencies.

## Compilation
To compile the contract, it is important that you have installed the project correctly, as we use external dependencies and contracts. Use the following command to compile the contracts: 
```
truffle compile
```

## Unit Tests
Since we build the [ERC-20](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20) smart contract on top of the audited [OpenZeppelin node modules](https://www.npmjs.com/package/@openzeppelin/contracts), there is no further requirement to write dedicated tests for these modules. Nonetheless, due to the fact that we integrate the non-standard [`permit`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Permit-permit-address-address-uint256-uint256-uint8-bytes32-bytes32-) method, unit tests have been written for this specific extension.

You can run the tests with 
```
npx hardhat test
```

Furthermore, if you need to test the [`permit`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Permit-permit-address-address-uint256-uint256-uint8-bytes32-bytes32-) method on one of the live test networks, run the following command to generate the function parameters (assuming [Node.js](https://nodejs.org/en) is installed):
```
node .\scripts\sign-data.js
```

## Deployment
### Local Deployment With Ganache
To deploy the contract on your local Ganache blockchain, you must first install the software on your computer. Follow the installation [guide](https://www.trufflesuite.com/ganache).

Once you installed the local blockchain, you can create a workspace. This is described [here](https://www.trufflesuite.com/docs/ganache/workspaces/creating-workspaces).
> **Note:** We have observed that Truffle and Ganache do not use the same default RPC configuration. The easiest way to align is to adjust Ganache's server hostname, port, and network ID with Truffle's configurations (check the file [`truffle-config.js`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/truffle-config.js)).

Once you are setup, just run: 
```
truffle migrate --network development
```

### Rinkeby
To deploy the smart contract to [Rinkeby](https://rinkeby.etherscan.io), you need to preconfigure first some things:
1. Create a `secrets.json` file.
2. Create a [MetaMask Wallet](https://metamask.io) and paste the respective seedphrase into `secrets.json`. Make sure you got some ETH. You can get some [here](https://faucet.rinkeby.io).
3. Create a new [Infura project](https://infura.io) and copy the project key into `secrets.json`.
4. Create a [Etherscan](https://etherscan.io) account and copy the API key to `secrets.json`.
The file will look like the following (make sure to always [`.gitignore`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/.gitignore) it!):
```json
{
    "seedPhrase": "drip voice crush ...",
    "privateKey": "0c7342ea3cdcc0...",
    "owner": "0x3854Ca47Abc6...",
    "projectId": "a657e3934de84d...",
    "etherscanKey": "RQFAFV4DE1H75P..."
}
```

Now run the following command:
```
truffle migrate --network rinkeby
```

If the deployment was successful, you will get the final deployment result:

![Deployment Result](/assets/RinkebyDeploymentResult.png)

Copy the contract address and verify the contract right away so that you can interact with it. Run the following command:
```
truffle run verify Startfeld@<CONTRACTADDRESS> --network rinkeby
```

If the verification was successful, you will see a similar result as follows:
```bash
Verifying Startfeld@0xF89BDCC76C4757C8C036bdC49cF4acE6977D6e41
Pass - Verified: https://rinkeby.etherscan.io/address/0xF89BDCC76C4757C8C036bdC49cF4acE6977D6e41#contracts
Successfully verified 1 contract(s).
```

For more information, see [here](https://github.com/rkalis/truffle-plugin-verify).
> **Note 1:** The smart contract [`Startfeld.sol`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/contracts/Startfeld.sol) does include a `premint` functionality in the constructor that creates an initial amount of 80'000 tokens for the deployer (no need anymore to separately call `mint` after the deployment).

> **Note 2:** The smart contract [`Startfeld.sol`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/contracts/Startfeld.sol) does include the [`permit`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#ERC20Permit-permit-address-address-uint256-uint256-uint8-bytes32-bytes32-) method, which can be used to change an account's ERC20 allowance (see [`IERC20.allowance`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#IERC20-allowance-address-address-)) by presenting a message signed by the account. By not relying on [`IERC20.approve`](https://docs.openzeppelin.com/contracts/4.x/api/token/erc20#IERC20-approve-address-uint256-), the token holder account doesn't need to send a transaction, and thus is not required to hold Ether at all.

## Interaction
If you deployed the smart contract succefully, you are now able to interact with it.

### Local Interaction With the Truffle CLI
To start the Truffle JavaScript console, please run:
```
truffle develop
```

In the console, you can create an instance of the provided contract by typing:
```javascript
let i = await SantisGulden.deployed()
```

You can use the instance variable to call functions like symbol:
```javascript
i.symbol()
```

### Rinkeby
Go to the corresponding Etherscan link, e.g. https://rinkeby.etherscan.io/address/CONTRACTADDRESS#code. You are able to invoke READ and WRITE functions on the contract.

## Test Deployments
The smart contract [`Startfeld.sol`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/contracts/Startfeld.sol) has been deployed across all the major test networks:
### Version 1 (Without `permit` Functionality)
- **Rinkeby:** [0x5c8ef4edbcae9cdd7f1b7cfef1c5848c480e347c](https://rinkeby.etherscan.io/address/0x5c8ef4edbcae9cdd7f1b7cfef1c5848c480e347c)
- **Ropsten:** N/A
- **Kovan:** [0x8239Dc2efeB262189e11f98cA5246eEd5AA88D4E](https://kovan.etherscan.io/address/0x8239dc2efeb262189e11f98ca5246eed5aa88d4e)
- **Goerli:** N/A

### Version 2 (With `permit` Functionality)
- **Rinkeby:** [0xF89BDCC76C4757C8C036bdC49cF4acE6977D6e41](https://rinkeby.etherscan.io/address/0xF89BDCC76C4757C8C036bdC49cF4acE6977D6e41)
- **Ropsten:** [0x64e9465B3B965e10f8C093729c9280aF9f1D9648](https://ropsten.etherscan.io/address/0x64e9465B3B965e10f8C093729c9280aF9f1D9648)
- **Kovan:** [0x6B956f1f061d43721453fb8F13D34ddb2419c0fC](https://kovan.etherscan.io/address/0x6B956f1f061d43721453fb8F13D34ddb2419c0fC)
- **Goerli:** [0xB9A82CCf8840c8DD9C082bA7b80cf6226B371477](https://goerli.etherscan.io/address/0xB9A82CCf8840c8DD9C082bA7b80cf6226B371477)

## Production Deployments on SwissDLT
The smart contract [`Startfeld.sol`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/contracts/Startfeld.sol) has been deployed to the SwissDLT network with [Remix<sup>*</sup> ](http://remix.ethereum.org) and signed with the Startfeld hardware wallet (Ledger Nano S):
### Version 1 (Without `permit` Functionality)
- [SwissDLT Block Explorer](https://swissdlt.appswithlove.net)
- Contract creation transaction hash: [0xbb15196471fdf5349bfcbe24a057f10387a8d5fcb5396c0910f581a3a531a1b9](https://swissdlt.appswithlove.net/tx/0xbb15196471fdf5349bfcbe24a057f10387a8d5fcb5396c0910f581a3a531a1b9)
- **Contract address:** [0xfc9ad717cc2781c837024f6bb2634381007fb6c1](https://swissdlt.appswithlove.net/address/0xfc9ad717cc2781c837024f6bb2634381007fb6c1)
- **Contract admin:** [0xa4d7a3cbfe2923b57dc68dead6692c3410c5605b](https://swissdlt.appswithlove.net/address/0xa4d7a3cbfe2923b57dc68dead6692c3410c5605b)
- Contract Application Binary Interface (ABI): Can be downloaded from the [snippet](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/snippets/15). This file was copied from Remix after compilation.

### Version 2 (With `permit` Functionality)
- [SwissDLT Block Explorer](https://swissdlt.appswithlove.net)
- Contract creation transaction hash: TBD
- **Contract address:** TBD
- **Contract admin:** TBD
- Contract Application Binary Interface (ABI): Can be downloaded from the [snippet](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/snippets/14). This file was copied from Remix after compilation.
> **Note 1:** Make sure that you always copy the full smart contract ABI and not just one of the inherited interfaces!

> **Note 2:** Remix uses checksummed addresses for the `At Address` button and if it's invalid the button is disabled. Always use checksummed addresses with Remix! One way to handle this is by using [EthSum](https://ethsum.netlify.app). The checksum algorithm is laid out in full detail [here](https://github.com/ethereum/EIPs/blob/master/EIPS/eip-55.md). 

<sup>*</sup> Remix deployment configuration:
- **Version 1 (Without `permit` Functionality)**
  - Compiler: `0.6.7+commit.b8d736ae`;
  - Language: `Solidity`;
  - EVM Version: `compiler default`;
  - Enable optimization: `200`;
  - Only the smart contract [`Startfeld.sol`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/contracts/Startfeld.sol) was used for compilation and deployment. Remix imported the dependencies successfully (see [here](https://remix-ide.readthedocs.io/en/latest/import.html) how this works in the background with the `.deps` folder);
- **Version 2 (With `permit` Functionality)**
  - Compiler: `0.8.6+commit.11564f7e`;
  - Language: `Solidity`;
  - EVM Version: `compiler default`;
  - Enable optimization: `200`;
  - Only the smart contract [`Startfeld.sol`](https://gitlab.appswithlove.net/startfeld/startfeld-token-contract/-/blob/main/contracts/Startfeld.sol) was used for compilation and deployment. Remix imported the dependencies successfully (see [here](https://remix-ide.readthedocs.io/en/latest/import.html) how this works in the background with the `.deps` folder);

## Further References
[1] https://docs.openzeppelin.com/contracts/4.x/erc20

[2] https://github.com/rkalis/truffle-plugin-verify

[3] https://www.trufflesuite.com/ganache

[4] https://www.trufflesuite.com/truffle
